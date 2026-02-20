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
