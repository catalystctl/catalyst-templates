# Game Server Templates

This directory contains game server templates organized by genre.

## Directory Structure

- **`action/`** - Fast-paced action games
- **`adventure/`** - Adventure and exploration games
- **`fps/`** - First-Person Shooter games (CS:GO, Valorant, etc.)
- **`mmo/`** - Massively Multiplayer Online games
- **`moba/`** - Multiplayer Online Battle Arena games
- **`racing/`** - Racing and driving games
- **`rts/`** - Real-Time Strategy games
- **`sandbox/`** - Open-world sandbox games (Minecraft, Terraria, Garry's Mod, etc.)
- **`simulation/`** - Simulation games
- **`sports/`** - Sports games
- **`strategy/`** - Strategy games
- **`survival/`** - Survival games (Rust, ARK, 7 Days to Die, etc.)

## Adding a New Game Template

When adding a new game server template:

1. Determine the appropriate genre category
2. Create a new directory with the game name
3. Include the following files:
   - `README.md` - Game-specific documentation
   - `config.json` or `config.yml` - Catalyst panel configuration
   - Any required startup scripts
   - Default configuration files

## Template Requirements

Each game template should specify:
- Game name and version
- Minimum system requirements (CPU, RAM, disk space)
- Required ports and protocols
- Startup command and parameters
- Installation steps
- Update procedures
- Common configuration options

## Popular Game Examples

Some commonly requested game servers include:
- **Sandbox**: Minecraft (Java/Bedrock), Terraria, Garry's Mod
- **Survival**: Rust, ARK: Survival Evolved, 7 Days to Die, Valheim
- **FPS**: CS:GO, CS2, Insurgency, Squad
- **Strategy**: Factorio, Satisfactory
- **MMO**: WoW servers, Ragnarok Online
