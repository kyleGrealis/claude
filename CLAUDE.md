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
    │   ├── commit-message/       # Git commit format conventions
    │   └── personal-context/     # Kyle's background & preferences
    ├── templates/                # R project config templates
    │   ├── README.md             # Package README (with placeholders guide)
    │   ├── CITATION.md           # Citation format
    │   ├── NEWS.md               # Changelog template
    │   ├── _pkgdown.yml          # pkgdown site config
    │   ├── .lintr                # lintr config
    │   ├── .pre-commit-config.yaml
    │   └── R/                    # R script templates
    ├── agents/                   # Empty (removed verbose agents)
    ├── settings.json             # Claude Code settings (synced)
    └── settings.local.json       # Machine-specific (gitignored)
```

## Available Skills

- **commit-message**: Git commit message format and conventions
- **personal-context**: Kyle's background, expertise, communication preferences
