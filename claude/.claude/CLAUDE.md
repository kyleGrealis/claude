# Global Claude Code Preferences

## R Code Style

When writing R code, always follow these conventions:

### Line Length
- **90 character max** (enforced by lintr in most projects)
- Break long function calls across multiple lines
- Put each argument on its own line for calls with 3+ arguments

### Lambdas
- **Always use braces for multi-line lambdas**: `\(x) { ... }`
- Single-expression lambdas can omit braces: `\(x) x + 1`

### Conditionals
- Expand inline `if/else` when either branch is long
- Use braces for multi-line if statements

### Tidyverse NSE
- Use `!!rlang::sym(col_name)` for column references from variables
- Add `utils::globalVariables(c(...))` at file top to silence R CMD check notes

### Before Finishing R Work
- **Run `lintr::lint_dir("R/")` and `lintr::lint_dir("tests/")` before declaring done**
- Fix all lints before finishing

## Git

**NEVER run `git add`, `git commit`, or `git push` commands.** Kyle handles all git operations himself. You may provide commit messages when explicitly requested, but do not execute any git staging, commit, or push commands.
