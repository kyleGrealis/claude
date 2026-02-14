---
name: r-package-conventions
description: R package development conventions following usethis workflow, roxygen2 documentation standards, and CRAN requirements. Use when creating or maintaining R packages.
---

# R Package Development Conventions

## Package Goals

Every package should:
* Pass CRAN checks on win-builder
* Use semantic versioning (MAJOR.MINOR.PATCH)
* Include comprehensive tests
* Have clean, informative documentation

## Required Files

### Always Include
* **CITATION.md** - Citation metadata (template in `~/.claude/templates/`)
* **NEWS.md** - User-focused changelog (template in `~/.claude/templates/`)
* **README.md** - Package overview with examples
* **pkgdown/_pkgdown.yml** - pkgdown website config (template in `~/.claude/templates/`)
* **inst/CITATION** - R citation file for `citation("pkg")`
* **.lintr** - Linter configuration (template in `~/.claude/templates/`)
* **.pre-commit-config.yaml** - Pre-commit hooks (template in `~/.claude/templates/`)
* **.Rbuildignore** - Build exclusions (template in `~/.claude/templates/`)

### Optional
* **CLAUDE.md** - Project-specific instructions for Claude Code (never committed to CRAN)
* **data-raw/** - Data preparation scripts (in .Rbuildignore)

## pkgdown Website Standards

All packages use a standardized `pkgdown/_pkgdown.yml`. Use the template at
`~/.claude/templates/_pkgdown.yml` and replace the UPPERCASE placeholders.

### Required Sections (in order)
1. **url** - `https://www.kyleGrealis.com/PACKAGE_NAME/`
2. **development** - `mode: auto`
3. **repo** - GitHub URLs for home, source, issue, user
4. **template** - Bootstrap 5, `light-switch: true`, standard bslib
5. **home.sidebar** - `[links, license, community, citation, authors, dev]`
6. **navbar** - left: `[intro, reference, articles, news]`,
   right: `[search, lightswitch, github]`
7. **navbar.components** - "Get Started" as `intro`, Articles dropdown,
   Changelog, GitHub icon with `fab fa-github fa-lg`
8. **articles** - first group titled "Get Started" with `navbar: ~`
9. **reference** - grouped by purpose with title and desc
10. **footer** - `developed_by` / `built_with`
11. **authors** - link to kylegrealis.com, sidebar roles `[aut, cre]`

### Key Conventions
* "Get Started" vignette is the first navbar item (replaces home icon)
* All vignettes listed under `articles` with `navbar: ~` to suppress
  auto-navbar entries; only non-intro articles appear in the Articles dropdown
* bslib values: `font-size: 0.9rem`, `border-radius: 0.5rem`,
  `btn-border-radius: 0.25rem`
* No custom fonts or colors in the template (keep packages visually consistent)

## Documentation Standards (roxygen2)

### Function Documentation
When adding a new function, write clean roxygen2 documentation:

```r
#' Brief one-line description
#'
#' Longer description explaining what the function does and why you'd use it.
#'
#' @param param_name Description of parameter. Specify type, constraints,
#'   and defaults if applicable.
#' @param another_param Another parameter description.
#'
#' @return Description of return value. Specify type and structure.
#'
#' @examples
#' # Basic usage
#' result <- my_function(param = "value")
#'
#' # Advanced usage
#' result <- my_function(
#'   param = "value",
#'   another_param = TRUE
#' )
#'
#' @export
my_function <- function(param, another_param = FALSE) {
  # Implementation
}
```

### Documentation Requirements
* Explain all `@param` arguments with types and constraints
* Describe `@return` value structure
* Include practical `@examples`
* Add `@export` for user-facing functions
* Add `@keywords internal` for internal helpers

## Testing Standards (testthat)

### When Adding a New Function
Write tests that cover:
* **Input validation** - Test argument checking and error messages
* **Expected behavior** - Test correct outputs for valid inputs
* **Edge cases** - Test boundary conditions, NAs, empty vectors
* **Error handling** - Test that errors are raised appropriately

```r
test_that("my_function validates inputs", {
  expect_error(
    my_function(param = NULL),
    "param cannot be NULL"
  )
})

test_that("my_function returns expected output", {
  result <- my_function(param = "value")
  expect_type(result, "list")
  expect_named(result, c("field1", "field2"))
})
```

### Running Tests
* Use `devtools::test()` for interactive testing
* Use `devtools::check()` before commits
* Pre-commit hooks run tests automatically

## NEWS.md Guidelines

### What to Include
* New features users need to know about
* Bug fixes that affect user experience
* Breaking changes with migration guidance
* Deprecations with alternatives

### What to Exclude
* Internal refactoring
* Minor tweaks that don't affect users
* Changes to tests or documentation structure
* Implementation details

### Format
```markdown
# packageName (development version)

* New feature description (#123)
* Fixed bug with X (#124)

# packageName 0.2.0

## New Features
* Added `new_function()` for doing Y

## Bug Fixes
* Fixed issue where Z would fail (#120)

## Breaking Changes
* `old_function()` deprecated; use `new_function()` instead
```

## Semantic Versioning

* **MAJOR (X.0.0)** - Breaking changes, incompatible API changes
* **MINOR (0.X.0)** - New features, backwards-compatible
* **PATCH (0.0.X)** - Bug fixes, backwards-compatible

## usethis Workflow

Common usethis commands for package development:

```r
# Setup
usethis::create_package("packageName")
usethis::use_git()
usethis::use_github()

# Add components
usethis::use_r("function_name")
usethis::use_test("function_name")
usethis::use_package("dplyr")
usethis::use_pipe()

# Documentation
usethis::use_readme_md()
usethis::use_news_md()
usethis::use_pkgdown()

# Quality control
usethis::use_lintr()
usethis::use_pre_commit()
```

## CRAN Submission

### Pre-submission Checklist
* `devtools::check()` returns 0 errors, 0 warnings, 0 notes
* Test on win-builder: `devtools::check_win_devel()`
* Update NEWS.md with version
* Update DESCRIPTION version
* Update CITATION.md version
* Build pkgdown site: `pkgdown::build_site()`

### Submission
* User manually submits to CRAN
* Claude Code can help prepare documentation and test results
* Never automate CRAN submission

## Pre-commit & Linting

Every R package project should use:
* **.lintr** - Enforces code style (90 char lines, snake_case, native pipe)
* **.pre-commit-config.yaml** - Runs styler, lintr, and other checks before commits

Templates available in `~/.claude/templates/`

Before finishing any R work:
* Run `lintr::lint_dir("R/")`
* Run `lintr::lint_dir("tests/")`
* Fix all lints before declaring done
