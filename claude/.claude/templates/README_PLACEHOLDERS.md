# README Template Placeholders

Quick reference for filling in `README.md` template.

## Required Placeholders

| Placeholder | Example | Notes |
|-------------|---------|-------|
| `PACKAGE_NAME` | `nhanesdata` | Used throughout |
| `LIFECYCLE_STAGE` | `maturing` | experimental, maturing, stable, superseded, deprecated |
| `LIFECYCLE_COLOR` | `blue` | orange (experimental), blue (maturing/stable), grey (deprecated) |
| `LICENSE_TYPE` | `MIT` | MIT, GPL-3, etc. |
| `LICENSE_URL` | `https://opensource.org/licenses/MIT` | Full license URL |
| `PROBLEM_STATEMENT_AND_MOTIVATION` | First 2-3 paragraphs | What problem does this solve? |
| `SOLUTION_DESCRIPTION` | How package solves it | One sentence summary |
| `CRAN_STATUS` | `submitted for approval Feb. 18, 2026` or `available now` | Update when on CRAN |

## Optional Placeholders

| Placeholder | Example | Notes |
|-------------|---------|-------|
| `OPTIONAL_CALLOUT_NOTE` | Public dataset URL | Delete entire `> NOTE` line if not needed |
| `DEPENDENCY_PACKAGE` | `nhanesA` | Delete Acknowledgments section if not needed |
| `DEPENDENCY_PURPOSE` | Foundation for accessing NHANES | Why you're acknowledging them |
| Custom badges | Update NHANES workflow badge | Add/remove as needed |
| `DATASET_OR_FEATURE_COUNT` | `71 datasets` | Or features, functions, etc. |
| `CATEGORY_COUNT` | `two` | Write out small numbers |
| Important Notes | COVID exclusion | Delete section if not applicable |
| Direct Access | Arrow example | Delete section if not applicable |

## Code Example Placeholders

- `main_function()`, `helper_function()`, etc. - Replace with actual function names
- `ARGS`, `INPUT` - Replace with realistic example arguments
- Function table - Add/remove rows as needed

## Delete If Not Applicable

- **Acknowledgments** section (if no major dependency)
- **Available Datasets / Features** section (if not relevant)
- **Important Notes** section (if no caveats)
- **Direct Access** section (if package-only usage)
- Individual badges (e.g., custom workflow badges)

## Lifecycle Stages

- `experimental` (orange) - Early development
- `maturing` (blue) - Functional but evolving
- `stable` (blue) - Production-ready
- `superseded` (grey) - Replaced by better solution
- `deprecated` (grey) - No longer recommended
