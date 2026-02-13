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

## Git

**NEVER run `git add`, `git commit`, or `git push` commands.** Kyle handles all git operations himself. You may provide commit messages when explicitly requested, but do not execute any git staging, commit, or push commands.
