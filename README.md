# Claude Code Configuration

Minimal [Claude Code](https://claude.ai/code) config using [GNU Stow](https://www.gnu.org/software/stow/) for portability. Recently streamlined from ~1,600 lines to ~100 lines by removing verbose agents, skills, and plugins.

**Philosophy**: Global config should be lean. Use skills for frequently-needed context only. Project-specific docs belong in project `.claude/CLAUDE.md` files.

## Contents

**Skills**: `personal-context` (18 lines), `commit-message` (42 lines)
**Plugins**: `context7`, `cloudflare-workers`
**Templates**: R package docs (`README.md`, `CITATION.md`, `NEWS.md`, `_pkgdown.yml`) + configs (`.gitignore`, `.lintr`, `.pre-commit-config.yaml`)
**Global instructions**: R style, pre-commit/lintr workflow, git workflow, teaching mode

## Installation

```bash
git clone https://github.com/kyleGrealis/claude.git ~/claude
cd ~/claude && stow claude
```

Creates symlink: `claude/.claude/` → `~/.claude/`

## Customizing

1. Fork repo
2. Update `personal-context/SKILL.md` and `CLAUDE.md`
3. Modify `settings.json` plugins and templates as needed

Add skills via `claude/.claude/skills/my-skill/SKILL.md`:
```markdown
---
name: my-skill
description: When to use this.
---
Content (keep concise)
```

## Don't Include

- Project-specific `.claude/` dirs (belong in projects)
- Verbose docs (link externally)
- Machine-specific settings (use gitignored `settings.local.json`)
- Secrets

## Troubleshooting

```bash
# File exists conflict
mv ~/.claude ~/.claude.backup && stow claude

# Adopt existing files
stow --adopt claude && git diff
```

MIT License
