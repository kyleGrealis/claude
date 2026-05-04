---
name: r-package
description: Kyle's R package development standards (structure, errors, roxygen, tests, NEWS, pre-commit, CRAN release prep). Use when developing or modifying an R package -- editing files in R/, tests/testthat/, vignettes/, man/, or files like DESCRIPTION, NAMESPACE, NEWS.md, cran-comments.md, _pkgdown.yml.
---

# R Package Development -- Kyle's Standards

Extracted from froggeR (his CRAN package). Apply unless the project clearly diverges.

## Structure

```
R/                  source, ~10 files; one public function per file when reasonable
tests/testthat/     mirror source: test-<source>.R + helper-*.R for shared fixtures
vignettes/          .Rmd files built with knitr (NOT Quarto -- CRAN compatibility)
man/                roxygen-generated, never hand-edit
inst/               only non-template assets (e.g. WORDLIST). Don't bundle templates.
NEWS.md             user-facing changelog (see format below)
cran-comments.md    written before each CRAN submission
DESCRIPTION         see below
```

Don't add empty scaffolding directories.

## DESCRIPTION

- `Authors@R` uses full `person()` including `email`, `role = c("aut", "cre")`, `comment = c(ORCID = "...")`. Pull from `personal-context` skill.
- `Title`: sentence case, no period, fits one line.
- `Description`: 2-4 sentence paragraph that explains the *philosophy* and *primary entry point*, not a feature list.
- **Pin minimum versions** for dependencies you actually rely on: `cli (>= 3.0.0)`, `usethis (>= 2.2.0)`, `testthat (>= 3.0.0)`.
- Move test-only deps to `Suggests:` (e.g. `yaml` if only tests parse YAML).
- Always include: `URL`, `BugReports`, `License: MIT + file LICENSE`, `Encoding: UTF-8`, `Config/testthat/edition: 3`.

## Source conventions

- **Public/internal split**: public `write_*()` handles UI + validation; internal `create_*()` does the I/O. Tests can target either layer.
- **Internal helpers prefixed with `.`** (e.g. `.validate_and_normalize_path()`). Not exported.
- **Errors via `rlang::abort()` with typed `class`**: `rlang::abort("message", class = "<pkg>_<error_type>")`. Examples: `class = "froggeR_invalid_path"`, `class = "froggeR_file_error"`. This enables `expect_error(..., class = "...")` in tests.
- **Interactive guards**: gate `readline()`, editor opens, and prompts behind `if (interactive())`.
- **Paths**: `here::here()` inside projects; `normalizePath(path, mustWork = TRUE)` to validate user-supplied paths; `fs::*` for cross-platform file ops.

## Roxygen style

- Title in title case, no terminal period.
- Blank line between paragraphs in `@description` and `@details`.
- Use `\code{...}` for inline code, `\itemize{ \item ... }` for lists, `\link{fn}` for cross-references.
- `@examples` for interactive functions: wrap in `if (interactive()) ...`.
- Order: `@param`, `@return`, `@details`, `@examples`, `@seealso`, `@export`.
- After editing roxygen: `devtools::document()`.

## Testing (testthat 3)

- Mock with `local_mocked_bindings()`. **Never `mockery`.**
- Filesystem isolation: `withr::local_tempdir()`.
- Internal function access: `pkg:::.fn()`.
- Section dividers in test files: `# Tests for .fn() ----` (then 2+ dashes for fold support).
- Use typed error matching: `expect_error(..., class = "<pkg>_<error_type>")` rather than regex on message.
- Shared fixtures: `tests/testthat/helper-<topic>.R`.

## NEWS.md format

```
# pkgname X.Y.Z         (current dev, no date)
# pkgname X.Y.Z (YYYY-MM-DD)   (released)

## Breaking changes
## Enhancements
## Bug fixes
```

- Bullets use `*`, sentence-case, end with period.
- Acknowledge contributors inline: `Thanks to Name (@handle) for the suggestion.`
- Cite sources for design decisions when relevant (Marwick, Boettiger, Mullen 2018).

## Pre-commit hooks

Use the template at `~/.claude/templates/.pre-commit-config.yaml` (mirrors froggeR's): styler (tidyverse), lintr, parsable-R, no-browser-statement, no-debug-statement, check-added-large-files (200kb cap), end-of-file-fixer, plus a local `forbid-to-commit` hook blocking `.Rhistory` / `.RData` / `.Rds` / `.rds`.

## Build & release commands

- Document: `devtools::document()`
- Test: `devtools::test()` -- single file: `testthat::test_file("tests/testthat/test-<x>.R")`
- Check: `devtools::check()` -- target **0 errors, 0 warnings, 0 notes** before any release work
- Build pkgdown: `pkgdown::build_site()`
- Install locally: `devtools::install()`

## CRAN release prep

Before submitting:

1. Bump `Version:` in DESCRIPTION
2. Write the NEWS.md entry (move from `# pkg X.Y.Z` heading to dated form *only after* CRAN accepts)
3. Update README if user-facing API changed
4. Run `devtools::check()` -- must be clean
5. Write/update `cran-comments.md`
6. Track remaining release tasks as a checklist in the project's `CLAUDE.md`

## Scaffolding & template usage

Templates live in `~/.claude/templates/`. Copy them into a new package, then fill placeholders from project context (DESCRIPTION, git remote, the `personal-context` skill). **Don't ask Kyle for any value you can derive.**

**Template inventory:**

| File | Purpose | Pull placeholders from |
|------|---------|------------------------|
| `.lintr` | lintr config (90 char, snake_case, pipe-call, commented-code off) | none |
| `.pre-commit-config.yaml` | pre-commit hooks (uncomment `deps-in-desc` for packages) | none |
| `.Rbuildignore` | files excluded from R CMD build | none |
| `.gitignore` | R/Quarto/Claude/Gemini ignore | none |
| `_pkgdown.yml` | pkgdown site config | DESCRIPTION (Title), git remote (PACKAGE_NAME) |
| `README.md` | package README | see placeholder table below |
| `NEWS.md` | changelog scaffold | DESCRIPTION (Package, Version) |
| `CITATION.md` | citation block + BibTeX | DESCRIPTION (Title, Description, Version), `personal-context` skill (name, email, ORCID, website) |

**Order of operations** when scaffolding from scratch:

1. `usethis::create_package()` first (DESCRIPTION, NAMESPACE, R/, basic .Rbuildignore).
2. Copy templates listed above.
3. Fill placeholders from context.
4. `devtools::document()` then `devtools::check()`.

**README.md placeholder reference:**

| Placeholder | Source | Notes |
|-------------|--------|-------|
| `PACKAGE_NAME` | DESCRIPTION:Package | Used throughout |
| `LIFECYCLE_STAGE` | ask Kyle | experimental, maturing, stable, superseded, deprecated |
| `LIFECYCLE_COLOR` | derive from stage | orange (experimental), blue (maturing/stable), grey (deprecated/superseded) |
| `LICENSE_TYPE` / `LICENSE_URL` | DESCRIPTION:License | MIT -> https://opensource.org/licenses/MIT, GPL-3 -> https://www.gnu.org/licenses/gpl-3.0 |
| `PROBLEM_STATEMENT_AND_MOTIVATION` | ask Kyle | 2-3 paragraphs on what the package solves |
| `SOLUTION_DESCRIPTION` | derive from PROBLEM_STATEMENT | one sentence |
| `CRAN_STATUS` | ask Kyle / check CRAN | "available now" or "submitted YYYY-MM-DD" |
| `OPTIONAL_CALLOUT_NOTE` | optional | delete the `> NOTE` line if not needed |
| `DEPENDENCY_PACKAGE` / `DEPENDENCY_PURPOSE` | optional | delete Acknowledgments section if no major dep |
| `main_function()`, `helper_function()`, etc. | NAMESPACE / R/ | replace with actual exported function names |
| `ARGS`, `INPUT` | derive from function signatures | realistic example values |

**Sections to delete from README.md if not applicable:** Acknowledgments (no major dep), Important Notes (no caveats), Related Packages (none), Direct Access (package-only usage), individual badges that don't apply.
