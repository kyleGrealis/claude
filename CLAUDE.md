# CLAUDE.md

This is a portable Claude Code configuration managed with GNU Stow for deployment across multiple machines. The `claude/` directory is the stow package that symlinks to `~/.claude`.

## Stow Commands

```bash
# Create symlinks (from repo root)
stow claude

# Remove symlinks
stow -D claude

# Adopt existing files into repo
stow --adopt claude
```

## Structure

```
claude/
└── .claude/
    ├── skills/
    │   └── personal-context/     # Kyle's background & preferences
    ├── settings.json             # Claude Code settings (synced)
    └── settings.local.json       # Machine-specific (gitignored)
```

## Available Skills

- **personal-context**: Kyle's background, expertise, communication preferences
