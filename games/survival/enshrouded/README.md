# Enshrouded (Catalyst)

A dedicated server template for [Enshrouded](https://enshrouded.com/) — survival,
crafting and Action-RPG combat on a sprawling voxel continent — with the 1.0
launch breakage fixed.

## Why this template exists

Enshrouded ships a **Windows-only** dedicated server (Steam app `2278520`,
`oslist: windows`), so Linux hosts have to run it through Proton. The
upstream Pterodactyl egg pins `ghcr.io/ptero-eggs/steamcmd:proton`, and that
image — built **2026-06-22**, i.e. *before* 1.0 — cannot run the 1.0 server:

| Component | Stock egg | This template | Why it matters |
|---|---|---|---|
| Base image | `ghcr.io/ptero-eggs/steamcmd:proton` (Debian 12, glibc 2.36) | `ghcr.io/ptero-eggs/steamcmd:ubuntu_24.04` (Ubuntu 24.04, glibc 2.39) | GE-Proton 11 is a glibc-2.38+ build; it cannot load on bookworm |
| Proton | GE-Proton **10-34** (baked in) | GE-Proton **11-9** (downloaded at install) | `lsteamclient` in 10-34 exports `SteamUtils008/009/010` — **no `SteamUtils011`** |
| Vulkan | `libvulkan.so.1` **missing** | present in the base image | GE-Proton 11's `proton` script `dlopen`s `libvulkan.so.1` at import time |
| Display | none | `Xvfb :99` started before Proton | A headless Wine/Proton prefix needs an X display |

Enshrouded 1.0 requires **`SteamUtils011`**. With 10-34 the server dies inside
Steamworks init — *before it writes `enshrouded_server.log`* — so the only
symptom is a bare exit-code-1 crash loop:

```
rm: cannot remove './logs/enshrouded_server.log': No such file or directory
tail: cannot open './logs/enshrouded_server.log' for reading: No such file or directory
```

Those two lines appear on a **healthy first boot too** (`rm` has nothing to
delete yet). The real signal is that `tail … --pid=$ENS_PID` exits 1
immediately: with a live pid it retries forever, with a dead pid it returns at
once. A dead `ENS_PID` plus no log file is the Steamworks-init failure
signature.

Verify the missing export yourself:

```bash
# Stock image — 0 matches, this is the bug
docker run --rm --entrypoint sh ghcr.io/ptero-eggs/steamcmd:proton -c \
  'grep -c SteamUtils011 /usr/local/bin/files/lib/wine/x86_64-windows/lsteamclient.dll'
# 0

# GE-Proton 11-9 — the export Enshrouded 1.0 needs
tar -xzf GE-Proton11-9-x86_64.tar.gz
grep -c SteamUtils011 GE-Proton11-9-x86_64/files/lib/wine/x86_64-windows/lsteamclient.dll
# 258
```

Upgrading the prefix (`GE-Proton10-10` → `GE-Proton10-18`), the numeric-password
workaround, and `slotCount`/`reservedSlots` maths are all unrelated older
causes and will not help a 1.0 launch failure.

## Requirements

| | Recommended | Notes |
|---|---|---|
| CPU | 4 cores | 2 cores works, but world generation is slow |
| RAM | 4–6 GB | The 1.0 server is heavier than the 0.x betas |
| Disk | 30 GB | ~9 GB installed, plus savegames and the Proton prefix |
| Architecture | `x86_64` | GE-Proton 11-9 has no usable `aarch64` Wine build here |

## Ports

Enshrouded uses a **single UDP port** for both game and query traffic.

| Port | Default | Protocol |
|---|---|---|
| Game + Query | 15637 | UDP |

The game client connects to `<ip>:<queryPort>`.

## Configuration

`enshrouded_server.json` is generated at install and then kept in sync by the
panel from these variables:

| Variable | Default | Purpose |
|---|---|---|
| `ENS_PROTON_VERSION` | `GE-Proton11-9` | Proton release. **Must stay 11-x or newer.** Reinstall to change. |
| `SRV_NAME` | `My Enshrouded Server` | Name shown in the server list |
| `MAX_PLAYERS` | `16` | Player slots (1–16) |
| `SRV_PW` | `ChangeMe1` | Admin group password |
| `SRV_PW2` | `ChangeMe2` | Friend group password |
| `SRV_PW3` | `ChangeMe3` | Guest group password |
| `SRV_PW4` | `ChangeMe4` | Visitor group password |
| `AUTO_UPDATE` | `1` | Update from Steam before each start |
| `VALIDATE` | `0` | Full Steam validation on next start (~8 GB re-read) |

Everything else — `gameSettings`, `saveDirectory`, `logDirectory`, the group
permission flags — is edited directly in `enshrouded_server.json`.

> **Passwords must not be purely numeric.** Enshrouded silently ignores numeric
> passwords, leaving the group open to anyone. The template enforces a minimum
> length and rejects digit-only values.

### Server not showing in the server list?

A known upstream problem. Add the server as a favourite via `IP:queryport` in
the Steam server browser; it then appears at the top of the in-game list.

## Savegames

Savegames live in `./savegame` inside the server directory. Back them up before
a wipe — Enshrouded has no server-side rollback.

## Troubleshooting

**Crash loop with no log file at all.** Check the Proton version: it must be
`GE-Proton11-x` or newer. Reinstall the server after changing
`ENS_PROTON_VERSION`.

**`libvulkan.so.1: cannot open shared object file`.** The server is running on
an image without Vulkan. Set the container image back to
`ghcr.io/ptero-eggs/steamcmd:ubuntu_24.04`.

**`version 'GLIBC_2.38' not found`.** The image is too old for GE-Proton 11
(e.g. `steamcmd:proton` is Debian 12 / glibc 2.36). The install script refuses
to proceed on such an image — switch to the template's default image.

**Server starts, then exits after a few seconds.** Read
`./logs/enshrouded_server.log`; unlike the Steamworks failure above, this one
does write a log. Usually a bad password or an invalid `gameSettings` value in
`enshrouded_server.json`.

## Credits

The Proton-11 recipe follows [jsknnr/enshrouded-server PR
#153](https://github.com/jsknnr/enshrouded-server/pull/153) and the
Dockerfiles of
[indifferentbroccoli/enshrouded-server-docker](https://github.com/indifferentbroccoli/enshrouded-server-docker)
and
[mornedhels/enshrouded-server](https://github.com/mornedhels/enshrouded-server).
Original Pterodactyl egg by Red-Banana-Official, Vapok, QuintenQVD0 and gOOvER.
