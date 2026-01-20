---
name: r-style-guide
description: Kyle's R coding style guide with tidyverse conventions and personal preferences. Use when writing or reviewing R code to ensure consistency. Covers quotes, indentation, pipes, naming, and markdown formatting.
---

# Kyle's R Style Guide

## Quote Usage

**ALWAYS use single quotes** unless one of these exceptions applies:

1. **Regex expressions** — double quotes for patterns
2. **Strings with apostrophes** — `"Don't know"` not `'Don\'t know'`
3. **User-facing messages** — when message naturally contains apostrophes

```r
# GOOD
x <- 'simple string'
pattern <- "\\d{3}-\\d{4}"
message <- "User's input is invalid"

# BAD
x <- "simple string"
```

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
  filter(status == 'active') |>
  group_by(category) |>
  summarize(mean_value = mean(value, na.rm = TRUE))

# BAD
result <- data %>%
  filter(status == 'active')
```

## Assignment

- **Use `<-`** not `=` for assignment
- Exception: function arguments use `=`

## Spacing

- Space after commas: `c(1, 2, 3)`
- Spaces around operators: `x + y`, `x == 5`
- Space after `#` in comments
- No spaces around `::` or `:::`
- Space before opening brace: `if (x) {`

## Line Length

- **Maximum 90 characters**
- Break at logical points

## Markdown Lists — CRITICAL

Bulleted lists in markdown/Quarto **MUST have 2 trailing spaces** after each line:

```markdown
Introduction with 2 spaces at end.
- First item with 2 trailing spaces
- Second item with 2 trailing spaces

Next paragraph.
```

Without trailing spaces, rendering breaks.

## Naming Conventions

- **snake_case** for functions and variables
- **SCREAMING_SNAKE_CASE** for constants
- **PascalCase** for S3/S4 class names only
- No dots in function names (except S3 methods)

```r
# GOOD
calculate_summary_stats <- function(data, na_rm = TRUE) { }
MAX_ITERATIONS <- 1000

# BAD
calcSummaryStats <- function(data) { }
calc.stats <- function(data) { }
```

## Control Flow

```r
# GOOD
if (x > 10) {
  do_something()
} else {
  do_other_thing()
}

# One-liner OK if short
result <- if (x > 10) 'high' else 'low'
```

## Comments

- Space after `#`
- Explain WHY, not WHAT
- Use roxygen2 `#'` for documentation

## Red Flags to Avoid

- **`attach()`** — namespace conflicts
- **`setwd()`** — breaks reproducibility (use `here::here()`)
- **`=` for assignment** — use `<-`
- **`%>%`** — use `|>`
- **Double quotes for simple strings** — use single quotes
- **Vertical alignment** — never align arguments
- **Growing vectors in loops** — pre-allocate or vectorize
- **Missing `na.rm`** — be explicit
- **Hard-coded paths** — use `here::here()`

## Summary

Goal: **readable, maintainable code** following modern R conventions.

1. Favor readability over cleverness
2. Be consistent within a project
3. Write code you'll understand in 6 months
4. Document the WHY, not the WHAT
