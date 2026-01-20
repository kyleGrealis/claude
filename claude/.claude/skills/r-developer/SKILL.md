---
name: r-developer
description: R development patterns for writing functions, packages, and Shiny apps. Use when creating new R code, building packages, or developing applications. Covers efficiency patterns, error handling, and anti-feature-creep philosophy.
---

# R Development Patterns

## Core Philosophy

1. **Efficiency Over Elegance**: Write code that works well and runs fast. Favor readability and debuggability over clever one-liners.

2. **Anti-Feature-Creep**: Ruthlessly question every feature. Ask: "Does this solve the actual problem, or am I adding it because I can?" Strip away functionality that doesn't directly serve the stated need.

3. **Maintainable Code**: Write code you'll understand in 6 months. Document the WHY, not the WHAT.

## Error Handling Pattern

Use `rlang::abort()` for informative errors in packages:

```r
validate_input <- function(x) {
 if (!is.numeric(x)) {
   rlang::abort(
     c(
       '`x` must be numeric.',
       x = paste0('You provided a ', class(x)[1], '.')
     )
   )
 }
 if (length(x) == 0) {
   rlang::abort('`x` cannot be empty.')
 }
}

process_data <- function(x) {
 validate_input(x)
 # Process...
}
```

Validate inputs early. Fail fast with clear messages.

## Performance Patterns

**Vectorize when possible**:
```r
# GOOD — vectorized
result <- df$value[df$value > 10]

# BAD — loop with growing vector
result <- c()
for (i in 1:nrow(df)) {
 if (df$value[i] > 10) {
   result <- c(result, df$value[i])
 }
}
```

**Pre-allocate if looping**:
```r
result <- vector('numeric', n)
for (i in seq_len(n)) {
 result[i] <- compute(i)
}
```

**Profile before optimizing**: Don't optimize prematurely. Use `profvis::profvis()` to find actual bottlenecks.

## purrr Usage Philosophy

Use purrr thoughtfully — prioritize debuggability:

1. Add explanatory comment BEFORE the code
2. Document expected output type
3. Ask: "Would a for loop be clearer?" If yes → use the loop

**Prefer purrr when**:
- Mapping over multiple inputs (`map2()`, `pmap()`)
- Working with nested lists
- Functional clarity outweighs debugging complexity

**Prefer loops when**:
- Simple iteration with clear steps
- Debugging will be needed
- Intent is clearer with explicit iteration

```r
# purrr with explanation
# Map over datasets, calculate summary stats
# Returns: list of data frames
summary_list <- datasets |>
 purrr::map(\(df) {
   df |>
     summarize(
       mean = mean(value, na.rm = TRUE),
       n = n()
     )
 })
```

## Function Design

**Keep functions focused**: One function, one job. If a function needs extensive documentation to explain all its behaviors, it's doing too much.

**Return early on failure**:
```r
process_data <- function(x, threshold = 10) {
 if (length(x) == 0) return(NULL)
 if (all(is.na(x))) return(NA_real_)
 
 # Main logic for valid input
 x[x > threshold]
}
```

**Use sensible defaults**: Defaults should handle the 80% use case. Make the common case easy.

## What NOT to Do

- **Over-engineer**: No excessive abstraction or premature generalization
- **Feature creep**: No adding features that weren't requested
- **Clever tricks**: No code golf or unnecessarily complex solutions
- **Verbose docs**: No documenting obvious code behavior

## Output Standards

When writing code, provide:
- Clean, executable R code with minimal but meaningful comments
- Brief explanation of key decisions
- Working examples users can copy-paste

Avoid:
- Over-explaining obvious code
- Suggesting features beyond the request
- Complex abstractions for simple problems

## Related Skills

- **r-style-guide**: Coding style conventions (quotes, indentation, pipes)
- **r-package-conventions**: Package development workflow and CRAN requirements
- **shiny-patterns**: Shiny app architecture and reactivity
