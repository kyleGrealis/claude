---
name: project-structure
description: Guidelines for choosing between R package structure vs analysis project structure. Covers the R/ bootstrap pattern with underscore-prefixed files for analysis projects.
---

# R Project Structure Guidelines

## Two Project Types

### R Package Structure
Use when building **distributable, reusable code**:
+ Functions that will be shared across projects
+ Code destined for CRAN or internal package repository
+ Tools that need formal documentation and testing
+ Need `devtools::check()` and CRAN compliance

**Structure:**
```
package_name/
├── R/                    # Function definitions
├── tests/testthat/       # Test suite
├── man/                  # Generated documentation
├── pkgdown/              # pkgdown website config
│   └── _pkgdown.yml
├── DESCRIPTION           # Package metadata
├── NAMESPACE             # Export declarations
├── NEWS.md              # Changelog
└── README.md            # Package overview
```

See [r-package-conventions](r-package-conventions) skill for details.

### Analysis Project Structure
Use for **one-off analyses, reports, and research projects**:
+ Quarto/RMarkdown documents for reports
+ Exploratory data analyses
+ Publications and presentations
+ Project-specific analyses (not reusable code)

**Structure:**
```
project_name/
├── R/                    # Bootstrap scripts (underscore-prefixed)
│   ├── _load.R          # Entry point (source this in .qmd files)
│   ├── _libraries.R     # All library() calls
│   ├── _data_dictionary.R  # Variable labels for tables
│   └── ...other helper files
├── data/                # Raw and processed data
├── docs/                # Rendered output
├── *.qmd                # Quarto documents
├── _quarto.yml          # Quarto configuration
└── README.md            # Project overview
```

## Analysis Project Pattern: R/ Bootstrap

### Core Concept
**Centralize all setup in R/ directory** using underscore-prefixed files that serve as configuration and initialization scripts.

### The Underscore Prefix Convention
Files starting with `_` are **configuration/setup files**, not analysis scripts:
+ `_load.R` - Single entry point sourced by all .qmd files
+ `_libraries.R` - All package dependencies
+ `_data_dictionary.R` - Variable labels for gtsummary tables
+ `_functions.R` - Project-specific helper functions (optional)
+ `_plot_theme.R` - Custom ggplot2 themes (optional)

### Key Files

#### R/_load.R (Entry Point)
Template: `~/.claude/templates/R/_load.R`

**Purpose:** Single source() call for all Quarto documents. Bootstraps the R environment.

**Usage in .qmd files:**
```r
source(here::here("R", "_load.R"))
```

**What it does:**
+ Sources `_libraries.R` to load packages
+ Sources `_data_dictionary.R` to load variable labels
+ Configures sumExtras options (if using)
+ Sources any other project-specific setup files

**Why:** Keeps .qmd files clean by externalizing all dependencies and setup.

#### R/_libraries.R (Package Dependencies)
Template: `~/.claude/templates/R/_libraries.R`

**Purpose:** Consolidate ALL library() calls in one location.

**Pattern:**
```r
suppressPackageStartupMessages({
  library(froggeR)
  library(gtsummary)
  library(rUM)
  library(sumExtras)
  library(tidyverse)
})
```

**Rules:**
+ ALL library() calls go here, NEVER in .qmd files
+ Use suppressPackageStartupMessages() to reduce console noise
+ List packages alphabetically for easy scanning

#### R/_data_dictionary.R (Variable Labels)
Template: `~/.claude/templates/R/_data_dictionary.R`

**Purpose:** Maintains variable names and labels for automated table labeling.

**Pattern:**
```r
dictionary <- dplyr::tribble(
  ~Variable,     ~Description,
  "age",         "Age in years",
  "gender",      "Biological sex",
  "bmi",         "Body mass index (kg/m²)",
)
```

**Usage:** Works with `sumExtras::add_auto_labels()` for gtsummary tables.

**Benefits:**
+ Single source of truth for variable labels
+ Consistent labeling across all tables
+ Easy to update labels project-wide

### Optional Bootstrap Files

#### R/_functions.R
Project-specific helper functions that don't belong in a package:
```r
# Custom data cleaning
clean_demographics <- function(data) {
  data |>
    mutate(age_group = cut(age, breaks = c(0, 18, 65, Inf)))
}

# Table formatting helpers
format_pvalue <- function(p) {
  case_when(
    p < 0.001 ~ "<0.001",
    TRUE ~ as.character(round(p, 3))
  )
}
```

#### R/_plot_theme.R
Custom ggplot2 themes and color palettes:
```r
theme_publication <- function() {
  theme_minimal() +
  theme(
    text = element_text(family = "Arial", size = 12),
    plot.title = element_text(face = "bold"),
    legend.position = "bottom"
  )
}

# Color palettes
colors_um <- c(
  green = "#005030",
  orange = "#f47321",
  gray = "#f9f9f9"
)
```

### Workflow: Analysis Project Setup

1. Create project directory structure
2. Copy templates from `~/.claude/templates/`:
   + Copy `R/_load.R`
   + Copy `R/_libraries.R`
   + Copy `R/_data_dictionary.R`
3. Customize `_libraries.R` with project-specific packages
4. Add variables to `_data_dictionary.R` as needed
5. In each .qmd file, add: `source(here::here("R", "_load.R"))`
6. Never use library() in .qmd files

### Workflow: Quarto Document Template

```r
---
title: "Analysis Title"
author: "Kyle Grealis"
format: html
---

# Setup
source(here::here("R", "_load.R"))

# Analysis
data <- read_csv("data/input.csv")

# Your analysis here...
```

## Decision Tree: Which Structure?

**Building reusable functions for multiple projects?** → Package structure
**Publishing to CRAN?** → Package structure
**Need formal documentation and testing?** → Package structure

**One-off analysis or report?** → Analysis project structure
**Quarto/RMarkdown document?** → Analysis project structure
**Project-specific code?** → Analysis project structure

## Key Differences

| Aspect | Package | Analysis Project |
|--------|---------|------------------|
| **Code location** | R/ with function definitions | R/ with setup scripts |
| **Entry point** | Functions exported via NAMESPACE | source("R/_load.R") |
| **Dependencies** | DESCRIPTION file | R/_libraries.R |
| **Documentation** | roxygen2 + man/ | Comments in code |
| **Testing** | tests/testthat/ | Interactive validation |
| **Output** | Installable package | Reports/documents |

## Common Mistakes

**DON'T:**
+ Use library() calls in .qmd files (use R/_libraries.R)
+ Mix package structure with analysis structure
+ Use relative paths (use here::here())
+ Create a package for one-off analyses
+ Duplicate library() calls across multiple .qmd files

**DO:**
+ Use R/ bootstrap pattern for analysis projects
+ Keep .qmd files clean by sourcing R/_load.R
+ Use package structure for reusable functions
+ Maintain data dictionary for consistent labeling
+ Use here::here() for all file paths
