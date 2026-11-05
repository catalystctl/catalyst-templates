# VanillaCord

A patch for vanilla servers to work with BungeeCord's ip_forward setting.

This uses [ME1312's fork](https://github.com/ME1312/VanillaCord) of VanillaCord which has been updated for modern Minecraft.

## Notes

If you've set up Spigot, Paper or some other server with BungeeCord's IP forwarding you might know that you have to set online-mode to false. Due to the way that VanillaCord works this is not necessary with this Egg, you can (and should) leave online-mode as true.

## Server Ports

The minecraft server requires a single port for access (default 25565) but plugins may require extra ports to enabled for the server.

| Port  | default |
|-------|---------|
| Game  | 25565   |

## Catalyst maintained template

This is a Catalyst-maintained copy of the Pterodactyl VanillaCord egg. The
upstream metadata and `support@pterodactyl.io` author attribution are retained
in `egg-vanilla-cord.json`.

### Changes from upstream

- The Ubuntu Jammy installer now declares and uses **bash** and installs
  prerequisites with **apt-get**, rather than invoking Alpine-only `ash`/`apk`.
- Metadata and JAR downloads use checked HTTPS requests and the official Mojang
  `version_manifest_v2.json` release list; Java, move and cleanup failures return
  nonzero instead of being hidden by the final `rm`.
- Official numeric two- and three-component release IDs are accepted unchanged.
  A separate three-component copy is used only for ordered artifact comparisons,
  so a release such as `26.3` is never sent to VanillaCord as `26.3.0`.
- Java 25 is the first/default runtime image. Java 21 and the legacy JDK images
  remain explicit choices through Catalyst's image selector (`IMAGE_VARIANT`),
  using names matching the `docker_images` keys; no unmapped `JAVA_VERSION`
  template variable is introduced.
- No blanket `set -e` is introduced.

### Configuration

| Variable | Default | Purpose |
|---|---|---|
| `SERVER_JARFILE` | `server.jar` | Patched server JAR used at startup |
| `VANILLA_VERSION` | `latest` | Official numeric `major.minor` or `major.minor.patch` release ID, or `latest` |

The default Java 25 runtime satisfies the current official `latest` release's
Mojang `javaVersion.majorVersion` requirement. When selecting an older release,
use the image selector to choose a Java image meeting that release's declared
Mojang Java requirement; explicit legacy releases may use Java 8. Changing
`VANILLA_VERSION` alone does not change the already-selected runtime image.
Allow outbound HTTPS access to `launchermeta.mojang.com` and `src.me1312.net`.
Allocate at least 2 GB RAM (4 GB is recommended when used with a proxy network)
and 10 GB disk plus world growth.

If VanillaCord connects to BungeeCord/Velocity, also enable IP forwarding in the
proxy and use the patched server's generated forwarding configuration. The
native game port defaults to **25565**. Back up worlds and settings before a
reinstall or version change.

The generated server obeys Minecraft's EULA. Read `eula.txt` and accept it only
if you agree.

### Upstream source and license

Source: `minecraft/java/vanillacord/egg-vanilla-cord.json` and its adjacent
README in [pterodactyl/game-eggs](https://github.com/pterodactyl/game-eggs), at
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
