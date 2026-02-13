# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

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

- `claude/.claude/agents/` - Custom agents for specialized tasks (R development, testing, reviewing, documentation, commits)
- `claude/.claude/skills/` - Reusable knowledge/context for R development
- `claude/.claude/settings.json` - Claude Code settings (synced)
- `claude/.claude/settings.local.json` - Machine-specific settings (gitignored)

## Available Agents

| Agent | Purpose |
|-------|---------|
| `r-developer` | Writing R functions, packages, Shiny apps |
| `r-tester` | Creating testthat tests, evaluating test quality |
| `r-reviewer` | Code review with honest, direct feedback |
| `r-documenter` | roxygen2 docs, README, NEWS, vignettes |
| `r-commit` | Crafting meaningful git commit messages |

## Available Skills

| Skill | Purpose |
|-------|---------|
| `personal-context` | Kyle's background, expertise, communication preferences |
| `r-style-guide` | R coding style (double quotes, `|>`, 2-space indent, no vertical alignment) |
| `r-package-conventions` | usethis workflow, roxygen2, testthat standards |
| `shiny-patterns` | Traditional Shiny structure with bslib/shinywidgets |
| `skill-writer` | Creating new Agent Skills |

## Key Style Rules (from r-style-guide)

- **Quotes**: Double quotes (tidyverse default)
- **Pipe**: Native `|>` not `%>%`
- **Assignment**: `<-` not `=`
- **Indentation**: 2 spaces, arguments on new lines, NO vertical alignment
- **Line length**: Max 90 characters
- **Markdown lists**: 2 trailing spaces after each line
- **Linting**: Code must pass `lintr` and `styler`

## Adding New Agents

Create markdown file in `claude/.claude/agents/`:

```yaml
---
name: agent-name
color: '#HEX'
description: When to use this agent.
model: sonnet
tools: Read, Write, Edit, Bash, Grep, Glob
---

Agent instructions here...
```

## Adding New Skills

Create directory in `claude/.claude/skills/skill-name/` with `SKILL.md`:

```yaml
---
name: skill-name
description: When Claude should use this skill.
---

# Skill content...
```
