# Starmade Server

## From their [Homepage](https://starmadedock.net/) - [Guide](https://www.star-made.org/help/setting_up_a_server)

The ultimate space sandbox. Participate in epic fleet battles, form alliances, strive to dominate entire galaxies and harness the universe’s resources for your industrious empire or the destruction of others. Customise your experience, the universe is yours!

## Server Ports

Ports required to run the server

| Port    | default |
|---------|---------|
| Game    | 4242    |

## Catalyst maintained template

This is a Catalyst-maintained copy of the Pterodactyl Starmade egg. The upstream
metadata and `contact@hunters-tavern.de` author attribution are retained in
`egg-starmade.json`.

### Changes from upstream

- The Temurin Focal installer explicitly installs `wget` before using it, and
  downloads the starter over verified HTTPS.
- Downloads, Java installation, the `StarMade` file move and cleanup all fail
  nonzero instead of ending in a success banner after an error.
- Server-directory setup is checked; no blanket `set -e` is introduced.

### Configuration and requirements

`SERVER_JARFILE` defaults to `StarMade.jar`, and the startup uses
`SERVER_MEMORY` and the allocated `SERVER_PORT`. Create the server with TCP/UDP
port **4242** as required by your network setup. Start with at least 2 GB RAM
(4 GB recommended for larger worlds); reserve 10 GB or more for the installation
and future data.

The installer uses `files.star-made.org`. Back up the server directory before
reinstalling, because the stock install recipe replaces the generated game tree.

### Upstream source and license

Source: `starmade/egg-starmade.json` and its adjacent README in
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
