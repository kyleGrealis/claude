# Kyle's R Preferences

## Style (tidyverse)
- Double quotes everywhere (tidyverse default)
- Arguments on new lines, 2-space indent, NO vertical alignment
- Max 90 characters per line
- Markdown lists: 2 trailing spaces after each line
- Code must pass `lintr` and `styler` checks
- **No emdashes (—) anywhere** in outward-facing docs, code comments, or user-visible strings. Rewrite sentences to avoid them.

## R Code
- Use `|>` not `%>%`
- Use `rlang::abort()` for package errors
- tidyverse/tidymodels default
- Max 2 solutions when troubleshooting

## Git
- **Never run `git add`, `git commit`, or `git push` commands.** Kyle handles all git operations himself. Only craft commit messages when asked.

## Communication
- One troubleshooting step at a time
- Include URLs/DOIs for references
- Define acronyms
- Say "I don't know" if unsure
