---
name: r-style-guide
description: Kyle's R coding style guide following tidyverse conventions. Use when writing or reviewing R code to ensure consistency. Covers quotes, indentation, pipes, naming, and markdown formatting.
---

# Kyle's R Style Guide

## Quote Usage

**ALWAYS use double quotes** unless one of these exceptions applies:

1. **Strings with double quotes inside** — `'She said "hello"'` not `"She said \"hello\""`

## Function Arguments & Indentation

**Arguments MUST start on a new line** after opening parenthesis. Never vertically align.

```r
# GOOD
result <- function_name(
  argument_1,
  argument_2,
  argument_3
)

# Also acceptable for very short calls
result <- function_name(arg1, arg2)

# BAD — vertical alignment
result <- function_name(argument_1,
                        argument_2)
```

**Multi-line formulas** — proper indentation:
```r
# GOOD
formula <- outcome ~ var1 + var2 +
  var3 + var4

# BAD
formula <- outcome ~ var1 + var2 +
                     var3 + var4
```

## Indentation Rules

- **2 spaces** per level (never tabs)
- Continuation lines indent by 2 spaces
- Pipe chains: indent each step by 2 spaces
- No trailing whitespace

## Pipe Usage

- **Use native pipe `|>`** not `%>%`
- One operation per line
- Start pipe on same line as object

```r
# GOOD
result <- data |>
  filter(status == "active") |>
  group_by(category) |>
  summarize(mean_value = mean(value, na.rm = TRUE))

# BAD
result <- data %>%
  filter(status == "active")
```

- **Maximum 90 characters** per line

## Linting & Formatting

- All R code must pass `lintr::lint_package()` checks
- All R code should be formatted with `styler::style_pkg()`
- Pre-commit hooks enforce both in projects that use them

## Markdown Lists — CRITICAL

Bulleted lists in markdown/Quarto **MUST have 2 trailing spaces** after each line:

```markdown
Introduction with 2 spaces at end.
- First item with 2 trailing spaces
- Second item with 2 trailing spaces

Next paragraph.
```

Without trailing spaces, rendering breaks.
