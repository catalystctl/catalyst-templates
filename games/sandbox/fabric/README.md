# Fabric

Fabric is a modular, lightweight mod loader for Minecraft.

[Fabric Website](https://fabricmc.net/)

## Server Ports

The fabric server requires a single port for access (default 25565) but plugins may require extra ports to enabled for the server.

| Port  | default |
|-------|---------|
| Game  | 25565   |

## Catalyst maintained template

This is a Catalyst-maintained copy of the Pterodactyl Fabric egg. The upstream
metadata, variables and `accounts@bofanodes.io` author attribution are retained
in `egg-fabric.json`.

### Changes from upstream

- Package setup and `/mnt/server` setup fail explicitly.
- Metadata requests use HTTP-failure detection, non-empty JSON results and
  checked pipelines; failed installers, downloads, renames and the final
  properties write return nonzero instead of reaching `Install Complete`.
- The script deliberately does **not** enable blanket `set -e`; ordinary shell
  compatibility remains unchanged.

### Configuration

| Variable | Default | Purpose |
|---|---|---|
| `SERVER_JARFILE` | `server.jar` | Startup JAR name |
| `MC_VERSION` | `latest` | Minecraft release, or `snapshot` |
| `FABRIC_VERSION` | `latest` | Fabric installer version |
| `LOADER_VERSION` | `latest` | Loader release, or `snapshot` |

Choose the runtime Java image for the selected Minecraft version. Install
downloads and metadata come from `maven.fabricmc.net` and `meta.fabricmc.net`.
Allow outbound HTTPS access to those hosts. Allocate at least 2 GB RAM (4 GB is
recommended for modded play) and 10 GB disk, plus space for every mod and world.

The generated server obeys Minecraft's EULA. Read `eula.txt` and accept it only
if you agree. Back up world, mod and configuration files before reinstalling or
changing Minecraft, loader or Java versions.

### Upstream source and license

Source: `minecraft/java/fabric/egg-fabric.json` and its adjacent README in
[pterodactyl/game-eggs](https://github.com/pterodactyl/game-eggs), at
[`c637a6d1e0449b167efeff81bb9a0177aa3df6c2`](https://github.com/pterodactyl/game-eggs/commit/c637a6d1e0449b167efeff81bb9a0177aa3df6c2).
The following upstream MIT notice applies to the copied/modified portions:

```text
MIT License

Copyright (c) 2024 Pterodactyl team and contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
