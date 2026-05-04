# Global Claude Code Preferences

## Fleet & Machine Roles

- **archMitters**: Daily-driver desktop. Arch Linux + Cosmic (Wayland). Only GUI machine -- graphical apps live here (Positron, Ghostty, LibreOffice, OnlyOffice, Proton Mail). Don't suggest GNOME/KDE.
- **pi5**: Headless Arch ARM server. Hosts NanoClaw + Tailscale main exit. SSH-only.
- **pi4**: Headless Arch ARM server. Hosts Shiny + static HTML. SSH-only.

Don't suggest desktop tools, GUI apps, or X11/Wayland configs for the Pis.

## How to work with Kyle

- Senior R/data-science engineer in academia / public health. Strong: R, tidyverse, tidymodels, package dev, Shiny. Currently learning: JS/Node, Cloudflare Workers, DevOps -- explain those briefly inline as you build.
- **Propose decisively, don't survey.** Lead with "I recommend X because Y." Only ask when the call is genuinely close.
- **Batch questions** in one `AskUserQuestion` call (1-4 per call). Don't drip-feed.
- **Don't ask permission for reversible actions** -- just do them and note what you did.
- One troubleshooting step at a time; max 2 solutions when stuck. Say "I don't know" if unsure.
- Include URLs/DOIs when citing references. Define acronyms on first use.
- **No emdashes anywhere** in user-facing text or code.

## File Operations

Use `trash` (trash-cli) for deletions -- never `rm` / `rm -rf`. `Bash(rm*)` is denied at the harness level; don't try to work around it. Use `git rm` for tracked files.

## Bash Command Style

Don't chain bash with `&&` / `;` / `|` or wrap in `for` / `while` loops -- the harness re-prompts on compound commands even when the inner parts are individually allowlisted. Use multiple single-command Bash calls back-to-back (parallel where independent). For repo-scoped git, use `git -C <path> <cmd>`. Atomic exception OK: `mkdir -p X && cp file X/`.

## Git

Default: **never** run `git add` / `git commit` / `git push`. Draft messages with `/commit-message`; let Kyle stage and commit.

**Trusted repos** (auto-allowed for full git, including push) -- use `git -C <path> <cmd>` form so the machine permission rule matches:

- `/home/kyle/claude`
- `/home/kyle/nanoclaw`
- `/home/kyle/dotfiles`
- `/home/kyle/dev/claude-memory-compiler`
