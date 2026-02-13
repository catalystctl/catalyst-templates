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

Each template should be in its own directory and include:

```
game-or-app-name/
├── README.md                 # Documentation
├── config.json or .yml       # Catalyst configuration
├── server.properties         # Game/app configuration (if applicable)
└── scripts/                  # Any startup or management scripts
    ├── install.sh
    ├── start.sh
    └── update.sh
```

### 3. Required Files

#### README.md
Every template must include a README.md with:
- Brief description of the game/application
- System requirements (CPU, RAM, disk space)
- Port requirements
- Installation instructions
- Configuration guide
- Common issues and troubleshooting

#### Configuration File
Include a Catalyst panel configuration file (JSON or YAML) with:
- Server name and description
- Startup command
- Port mappings
- Environment variables
- Resource limits
- File mounts

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
- [ ] README.md is complete and accurate
- [ ] Configuration file is properly formatted
- [ ] All file paths are relative to the template directory
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
