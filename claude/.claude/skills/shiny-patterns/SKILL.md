---
name: shiny-patterns
description: Shiny application development patterns with bslib, shinyWidgets, and htmltools. Use when building Shiny apps, dashboards, or reactive interfaces. Covers app structure, reactivity, UI design, and performance optimization.
---

# Shiny Application Patterns

## Preferred App Structure

Traditional multi-file structure (not golem/rhino unless complexity demands):

```
app-name/
├── app.R
├── R/
│   ├── ui_components.R
│   ├── server_logic.R
│   └── utils.R
├── www/
│   └── styles.css
├── data/
└── README.md
```

## app.R Structure

```r
library(shiny)
library(bslib)
library(dplyr)

ui <- page_navbar(
  title = 'App Name',
  theme = app_theme,

  nav_panel(
    'Main',
    layout_sidebar(
      sidebar = sidebar(
        # Inputs
      ),
      # Content
    )
  )
)

server <- function(input, output, session) {
  # Reactive values, expressions, outputs
}

shinyApp(ui, server)
```

## UI with bslib

```r
app_theme <- bs_theme(
  version = 5,
  bootswatch = 'flatly',
  base_font = font_google('Roboto')
)

ui <- page_navbar(
  title = 'App',
  theme = app_theme,

  nav_panel(
    'Analysis',
    layout_sidebar(
      sidebar = sidebar(
        selectInput('var', 'Variable', choices = vars),
        actionButton('run', 'Run')
      ),
      card(
        card_header('Results'),
        plotOutput('plot')
      )
    )
  )
)
```

## shinyWidgets Enhancements

```r
library(shinyWidgets)

pickerInput(
  'variable',
  'Select variables',
  choices = vars,
  multiple = TRUE,
  options = pickerOptions(liveSearch = TRUE, actionsBox = TRUE)
)

prettySwitch('show_outliers', 'Show outliers', value = TRUE, status = 'primary')
```

## Reactivity Patterns

**Reactive values**:
```r
rv <- reactiveValues(data = NULL, results = NULL)

observeEvent(input$load, {
  rv$data <- read_data()
})
```

**Reactive expressions** (cached):
```r
filtered_data <- reactive({
  req(rv$data)
  rv$data |> filter(year >= input$year_min)
})
```

**Event reactive** (button-triggered):
```r
results <- eventReactive(input$run, {
  expensive_computation(filtered_data())
})
```

## Performance Optimization

**Minimize reactivity**:
```r
# BAD — recalculates on every input change
output$table <- renderTable({
  data |> filter(cat == input$cat) |> summarize(mean = mean(val))
})

# GOOD — only on button click
summary_data <- eventReactive(input$calculate, {
  data |> filter(cat == input$cat) |> summarize(mean = mean(val))
})
output$table <- renderTable({ summary_data() })
```

**Debounce rapid inputs**:
```r
year_debounced <- reactive({ input$year }) |> debounce(500)
```

**Pre-process data outside server**:
```r
# Load once at startup
processed_data <- readRDS('data/clean.rds')

server <- function(input, output, session) {
  # Use pre-processed data
}
```

## Error Handling

```r
output$plot <- renderPlot({
  req(input$variable)

  validate(
    need(nrow(data()) > 0, 'No data available'),
    need(input$variable %in% names(data()), 'Invalid variable')
  )

  ggplot(data(), aes(!!sym(input$variable))) + geom_histogram()
})
```

## User Feedback

```r
showNotification('Data loaded', type = 'message', duration = 3)

withProgress(message = 'Processing...', value = 0, {
  for (i in 1:10) {
    incProgress(1/10, detail = paste('Step', i))
    Sys.sleep(0.1)
  }
})
```

## Modules (When Needed)

Use only when complexity justifies:

```r
# Module UI
filter_ui <- function(id) {
  ns <- NS(id)
  tagList(
    selectInput(ns('var'), 'Variable', choices = NULL)
  )
}

# Module server
filter_server <- function(id, data) {
  moduleServer(id, function(input, output, session) {
    observe({
      updateSelectInput(session, 'var', choices = names(data()))
    })
    reactive({ data() |> filter(!!sym(input$var) > 0) })
  })
}
```

## Anti-Patterns to Avoid

- Expensive computation in render functions (use reactive expressions)
- Missing `req()` for conditional dependencies
- `observe()` when `observeEvent()` is clearer
- Growing reactive values without limits

## Key Principles

1. Traditional structure for most apps
2. bslib for modern UI
3. Minimize reactivity — use `eventReactive()` for expensive operations
4. Cache with reactive expressions
5. Validate inputs with `req()` and `validate()`
6. Pre-process data outside server when possible
7. Modules only when complexity justifies it
