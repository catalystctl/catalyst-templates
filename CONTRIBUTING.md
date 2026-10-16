# Contributing to Catalyst Templates

Thank you for your interest in contributing to the Catalyst Templates repository! This guide will help you add new templates or improve existing ones.

## 📋 Before You Begin

1. Check if a template already exists for your game/application
2. Identify the correct category for your template
3. Review existing templates for structure and format examples

## 🎯 Adding a New Template

### 1. Choose the Right Category

Place your template in the appropriate directory:
- **Games**: Use the genre-specific subdirectory (`games/sandbox/`, `games/fps/`, etc.)
- **Databases**: Place in `databases/`
- **Voice Servers**: Place in `voice-servers/`
- **Web Servers**: Place in `web-servers/`
- **Storage**: Place in `storage/`
- **Bots**: Place in `bots/`
- **Other**: Use `misc/` for applications that don't fit elsewhere

### 2. Template Structure

A Catalyst template is a **single Pterodactyl-format egg JSON file** (`PTDL_v2`)
plus a `README.md`. There is no `config.json` or `scripts/` directory — the
install script is embedded in the egg as `scripts.installation.script`.

```
<category>/<game-or-app-name>/
├── egg-<slug>.json           # The template (PTDL_v2 egg)
└── README.md                 # Documentation
```

**The file name matters.** The panel's batch importer only discovers files whose
path matches `/egg[._-][^/]*\.json$/i`, so `egg-<slug>.json` is the safe
convention. CI rejects anything else with a hard error.

See `games/survival/enshrouded/` and `web-servers/fastdl/` for complete,
working examples.

### 3. Required Files

#### README.md
Every template must include a README.md with:
- Brief description of the game/application
- System requirements (CPU, RAM, disk space)
- Port requirements
- Installation instructions
- Configuration guide
- Common issues and troubleshooting

#### The egg JSON
Required top-level keys (CI validates all of these):

| Key | Notes |
|---|---|
| `name` | Non-empty, unique in the panel |
| `startup` | The run command. May reference `{{SERVER_PORT}}` and any `variables[].env_variable` — CI fails on a `{{…}}` that is neither |
| `docker_images` | At least one non-empty `{label: image}` entry. The **first** entry becomes the default image |
| `config.stop` | A stop command or `^C` / `^X`-style signal. Use `^C` (→ `SIGINT`) for Wine/Proton servers so the game can save |
| `scripts.installation.script` | The install script. Must pass `shellcheck --severity=error` |
| `variables` | Array of panel variables (may be empty) |

Optional but useful:
- `config.startup.done` — console line that marks the server as started
- `config.files` — JSON-string mapping of config files to patch, using `find: {"key": "{{server.build.env.VAR}}"}`. Catalyst's agent applies these (`parser`: `json`, `yaml`, `ini`, `xml`, `properties`, `file`)
- `features` — e.g. `steam_disk_space`
- `file_denylist` — paths users may not edit

### 4. Documentation Standards

- Use clear, concise language
- Include code examples where helpful
- Document all configuration options
- Provide default values
- Note any dependencies or prerequisites
- Include troubleshooting section

## ✅ Quality Checklist

Before submitting your template, ensure:

- [ ] Template is placed in the correct category
- [ ] Egg file is named `egg-<slug>.json` (the panel importer pattern)
- [ ] README.md is complete and accurate
- [ ] Egg JSON is valid and has `name`, `startup`, `docker_images`, `config.stop`
- [ ] Every `{{VAR}}` in `startup` is either a panel builtin (`SERVER_PORT`, …) or declared in `variables`
- [ ] Install script passes `shellcheck --severity=error`
- [ ] Port numbers don't conflict with common services
- [ ] Resource requirements are specified
- [ ] Startup command has been tested
- [ ] Security considerations are documented
- [ ] Common issues are addressed in troubleshooting

## 🔒 Security Guidelines

- **Never** include hardcoded passwords or API keys
- Use environment variables for sensitive configuration
- Document security best practices
- Include firewall configuration recommendations
- Warn about publicly exposed services
- Recommend strong authentication methods

## 📝 Commit Message Format

Use clear, descriptive commit messages:

```
Add [Game/App Name] template for [Category]

- Brief description of what was added
- Any special notes or requirements
```

Example:
```
Add Minecraft Java Edition template for sandbox games

- Supports versions 1.19+
- Includes Paper server configuration
- Performance optimizations included
```

## 🧪 Testing Your Template

Before submitting:

1. Test the installation process
2. Verify the startup command works
3. Check that all ports are accessible
4. Confirm resource limits are appropriate
5. Test common configuration changes

## 🤔 Questions?

If you're unsure about:
- Where to place a template
- How to structure the configuration
- What information to include

Feel free to open an issue for discussion before submitting your template.

## 📜 License

By contributing, you agree that your contributions will be licensed under the same license as this repository (see LICENSE file).

## 🙏 Thank You!

Your contributions help make Catalyst a better game panel for everyone. We appreciate your time and effort!
