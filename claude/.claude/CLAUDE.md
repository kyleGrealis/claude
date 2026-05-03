# Global Claude Code Preferences

## R Style (Kyle-specific deviations)

- Use `|>` not `%>%`
- Use `rlang::abort()` for package errors
- **Double quotes everywhere** (exception: strings containing double quotes)
- **90 character line limit** (enforced by lintr)
- **No vertical alignment** of function arguments
- **No emdashes (—)** in any user-facing text
- **Before finishing**: Run `lintr::lint_dir("R/")` and `lintr::lint_dir("tests/")`
- **Project templates**: Use templates from `~/.claude/templates/` when adding config files to projects
- **Rhino projects**: Use `rhino::pkg_install()` / `rhino::pkg_remove()` for package management -- never raw `renv::install()` or `renv::remove()`. Rhino wraps renv and keeps `dependencies.R` in sync.

## Bash Command Style

**Never chain bash commands with `&&`, `;`, `|`, or wrap them in `for`/`while` loops.** Each shell construct that combines or iterates over commands gets re-classified by Claude Code's permission system as a compound command and re-prompts Kyle even when the inner commands are individually allowlisted. This is the single biggest source of permission-prompt friction.

**Instead:**
- Run multiple single-command Bash calls back-to-back (separate tool invocations).
- For repo-scoped git operations, use `git -C <path> <command>` rather than `cd <path> && git <command>`.
- For "do X for each of A, B, C", make N separate Bash calls, not one `for` loop.
- Use the dedicated tools (Read / Edit / Write / Glob / Grep) instead of bash scripts wherever they fit — they don't go through the bash gate at all.
- Only chain commands when they form one tightly-coupled atomic operation (e.g. `mkdir -p X && cp file X/`), and even then prefer separate calls if reasonable.

This applies even when running commands in parallel via the multi-tool-call mechanism — those are still individual Bash invocations, no chaining inside any one of them.

## Communication

- One troubleshooting step at a time
- Max 2 solutions when troubleshooting
- Include URLs/DOIs for references
- Define acronyms on first use
- Say "I don't know" if unsure

## Teachable Moments

When working outside Kyle's core expertise (R/tidyverse), explain concepts inline as you build. Target the middle ground: skip obvious things, but don't wait until advanced topics to start explaining. Cover Cloudflare (Workers, D1, KV, R2 bindings, Wrangler), JavaScript/Node.js, web infrastructure (DNS routing, HTTP, APIs), and deployment concepts. Keep explanations brief and to the point.

## Pre-commit & Lintr Workflow

When pre-commit hooks fail or lintr reports issues:

1. **Fix the issues** based on error messages (don't skip hooks)
2. **Let user re-stage and commit** - NEVER run `git add` or `git commit`

**Common pre-commit failures**:
- styler formatting → already auto-fixed, just re-stage
- Large files (>200kb) → intentional or mistake?
- Debug statements (`browser()`, `debug()`) → remove before commit

## Git

**NEVER RUN** `git add`, `git commit`, or `git push`. Only check history with `git log` and draft commit messages when requested. User commits manually.

**Exceptions** (repos where git add/commit/push are allowed):
- `/home/kyle/claude`
- `/home/kyle/nanoclaw`
- `/home/kyle/dotfiles`
- `/home/kyle/dev/claude-memory-compiler`

For commit message format, use `/commit-message` skill.

## Teaching Mode

When a project enables teaching mode (indicated by `teaching_mode: true` in the project's `.claude/CLAUDE.md`):

1. **Check for a learning log** at `.claude/_learning-log.md` (or path specified in project config). This location ensures it's automatically gitignored since `.claude/` is typically in .gitignore.
2. **Adjust explanation depth based on topic category**:
   - **Mastered**: No explanations, just execute. Kyle knows this.
   - **Currently Learning**: Provide hints and guidance, not full explanations. Let Kyle figure it out with nudges.
   - **New/Fuzzy**: One sentence per new idea. For functions, packages, or arguments, be concise and include only the necessary bits to continue the project.
3. **Proactively suggest log updates**: When Kyle demonstrates understanding of a "Currently Learning" topic, suggest moving it to "Mastered". When he's clearly stuck on something "New", acknowledge it and keep it there.
4. **Default behavior**: If teaching mode is enabled but no learning log exists, ask Kyle whether to create a learning log or toggle off teaching mode. Don't proceed until this is resolved.

**Verbosity rule**: Teaching mode is NOT a tutorial. Keep it tight. No paragraphs, no over-explaining. Just enough to unblock and continue.

The learning log should be maintained collaboratively. Don't update it without Kyle's confirmation, but do suggest updates when progress is evident.
