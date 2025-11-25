# Claude Code Configuration with GNU Stow

A portable [Claude Code](https://claude.ai/code) configuration managed with [GNU Stow](https://www.gnu.org/software/stow/) for easy deployment across multiple machines.

## Why GNU Stow?

GNU Stow is a symlink farm manager that makes it easy to:

- **Sync configurations** across multiple machines via git
- **Keep dotfiles organized** in a single repository
- **Quickly deploy** your setup on new systems
- **Version control** your Claude Code customizations

Instead of copying files or maintaining separate configurations per machine, Stow creates symlinks from your home directory to this repository.

## Repository Structure

```
claude/                        # Repository root
├── claude/                    # Stow package directory
│   └── .claude/               # Symlinked to ~/.claude
│       ├── CLAUDE.md          # Global Claude Code instructions
│       ├── agents/            # Custom agents for specialized tasks
│       │   ├── r-commit.md
│       │   ├── r-developer.md
│       │   ├── r-documenter.md
│       │   ├── r-reviewer.md
│       │   └── r-tester.md
│       └── skills/            # Reusable knowledge/context
│           ├── personal-context/
│           ├── r-package-conventions/
│           ├── r-style-guide/
│           └── shiny-patterns/
├── .gitignore
├── LICENSE
└── README.md
```

### How Stow Works

The inner `claude/` directory is the "stow package." When you run `stow claude`, it creates symlinks as if the contents of that directory were in your home directory:

```
~/claude/claude/.claude  →  ~/.claude (symlink)
```

## Installation

### Prerequisites

- [GNU Stow](https://www.gnu.org/software/stow/) (`apt install stow`, `brew install stow`, `pacman -S stow`)
- [Claude Code](https://claude.ai/code) CLI

### Quick Start

```bash
# Clone the repository
git clone <your-repo-url> ~/claude

# Navigate to the repo
cd ~/claude

# Create symlinks
stow claude

# Verify
ls -la ~ | grep .claude
# Should show: .claude -> claude/claude/.claude
```

### On Additional Machines

```bash
# Clone and stow - that's it
git clone <your-repo-url> ~/claude
cd ~/claude && stow claude
```

## What's Included

### Agents

Agents are specialized Claude Code personas for specific tasks:

| Agent | Purpose |
|-------|---------|
| `r-developer` | Writing R functions, packages, and Shiny apps |
| `r-tester` | Creating and running testthat test suites |
| `r-reviewer` | Code review with honest, direct feedback |
| `r-documenter` | roxygen2 docs, README, NEWS, vignettes |
| `r-commit` | Crafting meaningful git commit messages |

### Skills

Skills provide persistent context that Claude Code can reference:

| Skill | Purpose |
|-------|---------|
| `personal-context` | Your background, expertise level, communication preferences |
| `r-style-guide` | Coding conventions and style preferences |
| `r-package-conventions` | Package development standards |
| `shiny-patterns` | Shiny app architecture patterns |

## Customizing for Your Own Use

### Fork and Modify

1. Fork this repository
2. Replace the agents and skills with your own
3. Update `personal-context` with your background and preferences
4. Commit and push

### Creating Your Own Agents

Create a markdown file in `claude/.claude/agents/`:

```markdown
---
name: my-agent
color: '#5D9CEC'
description: What this agent does and when to use it.
model: sonnet
tools: Read, Write, Edit, Bash, Grep, Glob
---

Your agent instructions here...
```

### Creating Your Own Skills

Create a directory in `claude/.claude/skills/` with a `SKILL.md` file:

```markdown
---
name: my-skill
description: When Claude should use this skill.
---

# Skill Name

Your skill content here...
```

## Managing Your Configuration

### Updating Across Machines

```bash
# On any machine
cd ~/claude
git pull
# Changes take effect immediately (symlinks!)
```

### Adding New Files

```bash
# Add to the stow package
cd ~/claude/claude/.claude/agents
# Create your new agent file

# Commit and push
cd ~/claude
git add -A && git commit -m "Add new agent" && git push
```

### Removing Symlinks

```bash
cd ~/claude
stow -D claude  # Deletes symlinks, preserves repo
```

## What NOT to Include

This repository is for **shared** configuration only. Keep these out:

- **Project-specific `.claude/` directories** - These belong in each project
- **Machine-specific settings** - Use `settings.local.json` (gitignored)
- **Sensitive information** - API keys, credentials, personal data
- **Conversation history** - Claude Code stores this elsewhere

## Troubleshooting

### Conflict: File Already Exists

If `~/.claude` already exists:

```bash
# Back up existing config
mv ~/.claude ~/.claude.backup

# Then stow
cd ~/claude && stow claude
```

### Stow Reports Conflicts

Stow won't overwrite existing files. Either remove them or use `--adopt` to pull them into the repo:

```bash
stow --adopt claude  # Moves existing files into repo
git diff             # Review what was adopted
```

## License

MIT License - see [LICENSE](LICENSE) for details.
