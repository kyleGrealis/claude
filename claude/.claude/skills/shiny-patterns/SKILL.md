---
name: shiny-patterns
description: Shiny application development patterns with emphasis on rhino framework. Covers app structure, modules, reactivity, and production best practices.
---

# Shiny Development Patterns

## Preferred Framework: rhino

**Use rhino for all new Shiny applications** unless explicitly told otherwise.

rhino provides:
* Modular architecture with dependency management
* Built-in testing infrastructure
* Code quality tools (lintr, styler) integration
* Development server with hot reload
* Production deployment patterns

### rhino Project Structure

```
app/
├── js/                    # JavaScript modules
├── logic/                 # Business logic (testable R functions)
│   └── __init__.R
├── static/                # Static assets (CSS, images)
├── styles/                # Sass/SCSS stylesheets
├── view/                  # UI modules
│   └── __init__.R
├── main.R                 # Application entry point
└── main.scss              # Main stylesheet

tests/
├── testthat/              # Unit tests
└── cypress/               # End-to-end tests

.lintr                     # Linter configuration
.rhino/                    # rhino configuration
config.yml                 # Application configuration
dependencies.R             # Package dependencies
renv.lock                  # Dependency lock file
```

### Starting a rhino Project

```r
# Create new rhino app
rhino::init("app_name")

# Development workflow
rhino::lint_r()           # Lint R code
rhino::lint_js()          # Lint JavaScript
rhino::lint_sass()        # Lint Sass/SCSS
rhino::test_r()           # Run R tests
rhino::test_e2e()         # Run Cypress tests
rhino::build_sass()       # Compile Sass to CSS
```

### rhino Module Pattern

**View Module** (`app/view/component.R`):
```r
box::use(
  shiny[...],
)

#' @export
ui <- function(id) {
  ns <- NS(id)

  tagList(
    h2("Component Title"),
    textInput(ns("input"), "Label"),
    textOutput(ns("output"))
  )
}

#' @export
server <- function(id) {
  moduleServer(id, function(input, output, session) {
    output$output <- renderText({
      input$input
    })
  })
}
```

**Logic Module** (`app/logic/calculations.R`):
```r
box::use(
  dplyr[...],
)

#' Calculate summary statistics
#' @export
calc_summary <- function(data) {
  data |>
    group_by(category) |>
    summarize(
      mean = mean(value, na.rm = TRUE),
      sd = sd(value, na.rm = TRUE)
    )
}
```

### Main Application (`app/main.R`)

```r
box::use(
  shiny[...],
  app/view/component,
)

#' @export
ui <- function(id) {
  ns <- NS(id)

  fluidPage(
    component$ui(ns("comp"))
  )
}

#' @export
server <- function(id) {
  moduleServer(id, function(input, output, session) {
    component$server("comp")
  })
}
```

## Traditional Shiny (Fallback)

If not using rhino, follow traditional structure:

```r
library(shiny)
library(bslib)

ui <- page_sidebar(
  title = "App Title",
  sidebar = sidebar(
    # Controls here
  ),
  # Main content here
)

server <- function(input, output, session) {
  # Server logic here
}

shinyApp(ui, server)
```

## UI Styling

### bslib for Bootstrap 5
Use bslib for modern, themeable UIs:

```r
box::use(
  bslib[...],
)

ui <- page_fillable(
  theme = bs_theme(
    preset = "bootstrap",
    primary = "#005030"
  ),
  # Content
)
```

### shinywidgets for Enhanced Inputs
Use shinywidgets for better input controls:

```r
box::use(
  shinywidgets[pickerInput, prettyCheckbox],
)

pickerInput(
  ns("picker"),
  "Select options:",
  choices = c("A", "B", "C"),
  multiple = TRUE,
  options = pickerOptions(
    actionsBox = TRUE,
    liveSearch = TRUE
  )
)
```

## Reactivity Patterns

### Use `reactive()` for Computed Values
```r
filtered_data <- reactive({
  req(input$filter)
  data |>
    filter(category == input$filter)
})
```

### Use `observe()` for Side Effects
```r
observe({
  # Update something based on input changes
  updateSelectInput(session, "select", choices = get_choices())
})
```

### Use `req()` for Input Validation
```r
output$table <- renderTable({
  req(input$data_source, input$date_range)
  # Render table
})
```

## Testing

### Testing Logic Functions
```r
# tests/testthat/test-calculations.R
box::use(
  app/logic/calculations[calc_summary],
  testthat[test_that, expect_equal],
)

test_that("calc_summary computes correct means", {
  data <- data.frame(
    category = c("A", "A", "B"),
    value = c(1, 2, 3)
  )

  result <- calc_summary(data)

  expect_equal(result$mean[result$category == "A"], 1.5)
  expect_equal(result$mean[result$category == "B"], 3.0)
})
```

### Running Tests
```r
# rhino projects
rhino::test_r()

# Traditional projects
devtools::test()
```

## Deployment Considerations

* Keep business logic in `app/logic/` (rhino) or separate R files (traditional)
* Test logic functions independently of Shiny reactive context
* Use `config.yml` for environment-specific settings
* Use renv for dependency management
* Consider shinyapps.io, Posit Connect, or Docker for deployment

## Resources

* rhino documentation: https://appsilon.github.io/rhino/
* bslib: https://rstudio.github.io/bslib/
* shinywidgets: https://dreamrs.github.io/shinywidgets/
