---
name: r-development
description: Kyle's R coding deviations from tidyverse style and his package finishing workflow. Use when writing or reviewing R, Rmd, or qmd files; working in R/, tests/, or vignettes/ directories; editing DESCRIPTION, .lintr, .pre-commit-config.yaml, _pkgdown.yml, NEWS.md, or CITATION files; or running lintr/styler/devtools/usethis.
---

# R Development -- Kyle's Deviations

Follow the tidyverse style guide by default. Below are Kyle's deviations and his workflow.

## Style deviations from tidyverse

- **Line length: 90 chars** (not 80). Enforced by lintr.
- **No emdashes anywhere** -- code, comments, roxygen, vignettes, README, NEWS.
- **`rlang::abort()`** for package errors (not `stop()`).

## Conventions Claude sometimes drifts from -- reinforce

- `|>` not `%>%`
- Double quotes everywhere (exception: strings containing double quotes)
- No vertical alignment of function arguments

## Finishing checklist

Before declaring R work done, run:

- `lintr::lint_dir("R/")`
- `lintr::lint_dir("tests/")`

Fix any output before handing back.

## Pre-commit / lintr failures

Fix the issue; never skip the hook. Common failures:

- styler reformatted -- just re-stage
- File >200 kb -- flag and ask if intentional
- `browser()` / `debug()` left in -- remove

Never run `git add` / `git commit` -- Kyle re-stages and commits.

## Rhino projects

Use `rhino::pkg_install()` / `rhino::pkg_remove()` only -- never raw `renv::install()` / `renv::remove()`. Rhino wraps renv and keeps `dependencies.R` in sync.

## Templates

For new R config files (.lintr, .pre-commit-config.yaml, _pkgdown.yml, README, NEWS, CITATION, .Rbuildignore, .gitignore), use templates from `~/.claude/templates/`.
