# Global Claude Code Preferences

## R Code Style

When writing R code, always follow these conventions:

### Quotes
- **Double quotes everywhere** (tidyverse default)
- Exception: Strings with double quotes inside — use `'She said "hello"'` not `"She said \"hello\""`

### Line Length
- **90 character max** (enforced by lintr in most projects)
- Break long function calls across multiple lines
- Put each argument on its own line for calls with 3+ arguments

### Indentation
- Arguments on new lines, 2-space indent
- **NO vertical alignment**

### Lambdas
- **Always use braces for multi-line lambdas**: `\(x) { ... }`
- Single-expression lambdas can omit braces: `\(x) x + 1`

### Conditionals
- Expand inline `if/else` when either branch is long
- Use braces for multi-line if statements

### Tidyverse NSE
- Use `!!rlang::sym(col_name)` for column references from variables
- Add `utils::globalVariables(c(...))` at file top to silence R CMD check notes

### Markdown Formatting
- Bulleted lists: 2 trailing spaces after each line
- **No emdashes (—) anywhere** in outward-facing docs, code comments, or user-visible strings. Rewrite sentences to avoid them.

### Before Finishing R Work
- **Run `lintr::lint_dir("R/")` and `lintr::lint_dir("tests/")` before declaring done**
- Fix all lints before finishing
- Code must pass `lintr` and `styler` checks

## R Code Patterns

- Use `|>` not `%>%`
- Use `rlang::abort()` for package errors
- tidyverse/tidymodels default approach

## Communication

- One troubleshooting step at a time
- Max 2 solutions when troubleshooting
- Include URLs/DOIs for references
- Define acronyms on first use
- Say "I don't know" if unsure

## Teachable Moments

When working outside Kyle's core expertise (R/tidyverse), explain concepts inline as you build. Target the middle ground: skip obvious things, but don't wait until advanced topics to start explaining. Cover Cloudflare (Workers, D1, KV, R2 bindings, Wrangler), JavaScript/Node.js, web infrastructure (DNS routing, HTTP, APIs), and deployment concepts. Keep explanations brief and to the point.

## Git

**NEVER run `git add`, `git commit`, or `git push` commands.** Kyle handles all git operations himself. You may provide commit messages when explicitly requested, but do not execute any git staging, commit, or push commands.

## Teaching Mode

When a project enables teaching mode (indicated by `teaching_mode: true` in the project's `.claude/CLAUDE.md`):

1. **Check for a learning log** (typically `_learning-log.md` in project root, or path specified in project config)
2. **Add learning log to .gitignore**: If the project is a git repository and the learning log file is not already in .gitignore, add it. This prevents tracking personal learning notes in version control.
3. **Adjust explanation depth based on topic category**:
   - **Mastered**: No explanations, just execute. Kyle knows this.
   - **Currently Learning**: Provide hints and guidance, not full explanations. Let Kyle figure it out with nudges.
   - **New/Fuzzy**: One sentence per new idea. For functions, packages, or arguments, be concise and include only the necessary bits to continue the project.
4. **Proactively suggest log updates**: When Kyle demonstrates understanding of a "Currently Learning" topic, suggest moving it to "Mastered". When he's clearly stuck on something "New", acknowledge it and keep it there.
5. **Default behavior**: If teaching mode is enabled but no learning log exists, ask Kyle whether to create a learning log or toggle off teaching mode. Don't proceed until this is resolved.

**Verbosity rule**: Teaching mode is NOT a tutorial. Keep it tight. No paragraphs, no over-explaining. Just enough to unblock and continue.

The learning log should be maintained collaboratively. Don't update it without Kyle's confirmation, but do suggest updates when progress is evident.
