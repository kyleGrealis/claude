---
name: commit-message
description: Draft git commit messages following Kyle's format conventions.
---

# Commit Message Format

## Workflow

1. Run `git status` to see staged/unstaged changes
2. Run `git diff --cached` (or `git diff` if nothing staged)
3. Run `git log --oneline -5` to see recent commit style
4. Analyze changes and draft commit message

## Format

```
type(scope): what it solved
  - indented bullet for minimal how
  - another indented bullet for maybe why
  - reference to source URL if applicable
```

**Types:** `feat`, `fix`, `docs`, `test`, `refactor`, `chore`

## Rules

- Keep first line under 72 characters
- Use bullets for body (not paragraphs)
- Include source URLs when relevant
- Focus on WHY, not just WHAT
- NEVER add `Co-Authored-By: Claude` attribution

## Example

```
fix(data import): resolved column type mismatch
  - Cast date strings to Date before merge
  - Issue was ISO format not recognized by lubridate
  - Solution from https://stackoverflow.com/questions/12345
```
