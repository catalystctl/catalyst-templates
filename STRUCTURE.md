# Template Repository Structure

This document provides a visual overview of the Catalyst Templates repository structure.

## Directory Tree

```
catalyst-templates/
│
├── README.md                          # Main repository documentation
├── CONTRIBUTING.md                    # Contribution guidelines
├── LICENSE                            # Repository license
│
├── games/                             # Game server templates
│   ├── README.md                      # Games category documentation
│   ├── action/                        # Action games (GTA V, etc.)
│   ├── adventure/                     # Adventure games
│   ├── fps/                           # First-person shooters (CS:GO, CS2, etc.)
│   ├── mmo/                           # MMO games (WoW, etc.)
│   ├── moba/                          # MOBA games (Dota 2, etc.)
│   ├── racing/                        # Racing games (Assetto Corsa, etc.)
│   ├── rts/                           # Real-time strategy (AoE, etc.)
│   ├── sandbox/                       # Sandbox games (Minecraft, Terraria, etc.)
│   ├── simulation/                    # Simulation games (Farming Sim, etc.)
│   ├── sports/                        # Sports games (Rocket League, etc.)
│   ├── strategy/                      # Strategy games (Factorio, etc.)
│   └── survival/                      # Survival games (Rust, ARK, etc.)
│       └── enshrouded/                # Enshrouded (Catalyst) — GE-Proton 11-9
│           ├── egg-enshrouded-catalyst.json
│           └── README.md
│
├── databases/                         # Database server templates
│   └── README.md                      # MySQL, PostgreSQL, MongoDB, Redis, etc.
│
├── voice-servers/                     # Voice communication templates
│   └── README.md                      # TeamSpeak, Mumble, etc.
│
├── web-servers/                       # Web server templates
│   ├── README.md                      # Apache, Nginx, Node.js, etc.
│   └── fastdl/                        # FastDL (Caddy) — sv_downloadurl content server
│       ├── egg-fastdl-caddy.json
│       └── README.md
│
├── storage/                           # Storage and file transfer templates
│   └── README.md                      # FTP, MinIO, Nextcloud, etc.
│
├── bots/                              # Bot application templates
│   └── README.md                      # Discord, Telegram, IRC bots, etc.
│
└── misc/                              # Miscellaneous templates
    └── README.md                      # Monitoring, media servers, proxies, etc.
```

## Template Organization Philosophy

### 1. **Genre-Based Game Organization**
Games are organized by genre to make it easier to find similar games and understand typical configuration patterns for each game type.

### 2. **Service Type Categories**
Non-game applications are categorized by their primary function (databases, storage, communication, etc.).

### 3. **Scalability**
The structure is designed to accommodate:
- Hundreds of game templates across different genres
- Multiple versions of the same game
- Modded or customized server configurations
- Both popular and niche applications

## Adding New Templates

When adding a new template:

1. **Identify the category** - Choose the most appropriate directory
2. **Create a subdirectory** - Name it after the game/application
3. **Name the egg file so the panel finds it** - the Catalyst batch importer only
   discovers paths matching `/egg[._-][^/]*\.json$/i`, so `egg-<slug>.json` is the
   safe convention (CI fails otherwise)
4. **Add documentation** - Include a README.md with setup instructions
5. **Include configurations** - Add all necessary config files
6. **Test thoroughly** - Ensure the template works as expected

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## Category Descriptions

| Category | Purpose | Examples |
|----------|---------|----------|
| **games/** | Game server templates | Minecraft, Rust, CS:GO |
| **databases/** | Database systems | MySQL, PostgreSQL, Redis |
| **voice-servers/** | Voice communication | TeamSpeak, Mumble |
| **web-servers/** | Web hosting | Apache, Nginx, Node.js |
| **storage/** | File storage/transfer | FTP, MinIO, Nextcloud |
| **bots/** | Bot applications | Discord bots, Telegram bots |
| **misc/** | Other applications | Monitoring, media servers |

## Future Expansion

The structure is designed to grow organically. As new categories of applications become popular, new top-level directories can be added following the same documentation patterns.
