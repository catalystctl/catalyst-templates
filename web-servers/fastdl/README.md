# FastDL (Caddy) — HL1/Source Content Server

A standalone Fast Download (FastDL) web server for Half-Life 1 and Source engine
games. When a game server has custom content (maps, models, sounds, materials)
that isn't shipped with the game, clients would normally download it from the
game server itself at a few KB/s, blocking the match. FastDL moves those
downloads to a plain HTTP server that the game client fetches directly, at full
line speed.

This template runs [Caddy](https://caddyserver.com)'s static file server — a
single static Go binary on a minimal Alpine image. No database, no scripting,
no dynamic content: roughly 64 MB of RAM even under load, and it is practically
impossible to misconfigure into serving something outside its docroot.

## Why Caddy's file server

Game engines are picky HTTP clients. Caddy's `file-server` passes all of the
checks that matter for `sv_downloadurl` (all verified against the binary this
egg installs):

| Requirement | Behavior |
|---|---|
| Byte-Range resumption | `Accept-Ranges: bytes`, correct `206 Partial Content` + `Content-Range` (engine retry/resume depends on it) |
| Conditional requests | `ETag` / `Last-Modified` → `304 Not Modified`, so rejoining players revalidate instead of re-downloading |
| Correct MIME types | Built-in database (`.bsp`, `.mdl`, `.wav`, `.bz2`, …) — engines are strict about content types |
| Many simultaneous clients | Configurable connection limit; internal file cache auto-scales; Go runtime holds thousands of concurrent streams |
| Restricted docroot | Symlink escapes and `../` traversal return `404` (verified) |
| HTTP/1.0 fallback | Older engines and some tools speak HTTP/1.0 — Caddy handles it fine |

Compression is deliberately disabled (`--no-compress`): FastDL payloads are
already-compressed binaries and HL1 engines request `.bz2` files themselves,
so on-the-fly gzip would only burn CPU. Optional `.bz2` pre-compression is
described below.

## Compatibility

Works with any game that supports `sv_downloadurl`, notably:

- Counter-Strike 1.6 (also via the bundled `counter_strike_1.6_rehlds` template)
- Garry's Mod (also via the bundled `gmod` template)
- Counter-Strike: Source, Team Fortress 2, Half-Life 2: Deathmatch, Day of
  Defeat: Source, Left 4 Dead 1/2, Sven Co-op, and other HL1/Source titles

## Installation & layout

After install, the server directory contains:

```
caddy            # static Caddy binary (installed)
fastdl/          # HTTP docroot — this is all that is served
├── README.txt   # quick-reference notes (auto-generated at install)
├── cstrike/     # seeded empty — CS 1.6 content goes here
└── garrysmod/   # seeded empty — GMod content goes here
```

Upload game content with SFTP, mirroring the game server's own layout **inside
a folder named after the game's mod directory**:

- CS 1.6: `fastdl/cstrike/maps/de_yourmap.bsp`
- GMod: `fastdl/garrysmod/models/player/...`
- Any other game: `fastdl/<gamedir>/...`

The root of the HTTP server **is** the `fastdl/` folder, so a client asking for
`cstrike/maps/de_yourmap.bsp` resolves to `fastdl/cstrike/maps/de_yourmap.bsp`.

## Hooking up a game server

On each game server (console, `server.cfg`, or the panel's startup/config
tools):

```
sv_downloadurl "http://<node-ip>:<this-server-port>"
sv_allowdownload 1        // HL1 default; ensure it is not disabled
```

- HL1 games (CS 1.6): the client requests `<gamedir>/<path>`, i.e.
  `cstrike/maps/...`
- Source games: the client requests `<path>` under the game's mod dir, i.e.
  `garrysmod/maps/...` — same layout rule, nothing extra to configure
- GMod additionally supports Workshop collections (`resource.AddWorkshop`),
  which bypasses FastDL entirely; FastDL is the right tool for loose files and
  legacy content

Run `changelevel` (or restart the map) after changing `sv_downloadurl` for it
to take effect. Verify from any machine:

```
curl -I http://<node-ip>:<port>/cstrike/maps/de_yourmap.bsp
```

Expect `HTTP/1.1 200 OK` with an `Accept-Ranges: bytes` header. A `404` means
the file is not under `fastdl/<gamedir>/` with the exact path clients request.

## Optional: cut bandwidth with .bz2

HL1-family engines automatically request `file.bz2` when both exist and
decompress it client-side — usually a 60–80% reduction for `.bsp`/`.mdl`
content. Generate alongside the original (keep both):

```
bzip2 -k9 fastdl/cstrike/maps/de_yourmap.bsp   # produces de_yourmap.bsp.bz2
```

## HTTPS / public hosting

HL1/Source FastDL is plain HTTP by protocol. To offer a branded or
TLS-protected URL, put a reverse proxy or CDN in front of this server; the
proxy/CDN **must forward Range requests** (most do). The game server then uses
the public URL in `sv_downloadurl` (Source also accepts a `##NN` speed-limit
suffix; HL1 ignores it).

## Variables

| Variable | Default | Purpose |
|---|---|---|
| `FD_CADDY_VERSION` | `2.11.4` | Caddy release to install; `latest` resolves at install time. Reinstall the server to upgrade. |
| `FD_MAX_CONNECTIONS` | `0` | Concurrent client connections accepted (`0` = unlimited; the file cache auto-scales). Set ~3× your expected simultaneous downloads if you want a hard cap. |

## Ports

Only the primary port is required — it serves HTTP and is the port game
servers reference in `sv_downloadurl`.

| Port | default |
|------|---------|
| HTTP (FastDL) | 27015 |
