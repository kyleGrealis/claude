---
name: recall
description: Search Kyle's personal knowledge base in ~/dev/claude-memory-compiler/ for prior lessons, decisions, or incidents. Use when Kyle asks "what did I figure out about X", "did we solve Y before", "remind me about Z" -- or proactively when current work touches a topic that may have a prior write-up (Quarto styling, backup systems, Docker proxy, systemd timers, etc.).
---

# Recall from Knowledge Base

The KB lives at `~/dev/claude-memory-compiler/`:

- `knowledge/index.md` -- markdown table indexing all distilled articles with one-line summaries.
- `knowledge/concepts/<slug>.md` -- distilled topic articles.
- `knowledge/connections/<slug>.md` -- cross-topic insights.
- `daily/<YYYY-MM-DD>.md` -- raw daily logs (richer than the index, larger).

## Workflow

1. **Scan the index first.** Read `~/dev/claude-memory-compiler/knowledge/index.md`. The Summary column is enough to filter.
2. **Read matching article(s) in full** -- they're short.
3. **If the index doesn't have it, grep daily logs** for the term: search recursively under `~/dev/claude-memory-compiler/daily/`, then read matches.
4. **Summarize back to Kyle**, citing the article path so he can open the source.

## When to use proactively

Without being asked, invoke this skill when current work touches a topic that has a clear KB index entry. Examples:

- Editing Quarto styling -> recall `quarto-output-location-column-scoping`, `quarto-cache-staleness-images`.
- Touching pre-commit / git ignore -> recall `git-cache-vs-gitignore`.
- Working with systemd timers / backups -> recall `systemd-timers-for-backups`, `three-layer-backup-system`.
- Provider-switching in LLM apps -> recall `ollama-session-corruption`, `provider-switching-patterns`.

Better to mention "I see we hit this before -- prior write-up at `<path>`" than to re-debug.

## What NOT to recall

- Obvious general knowledge ("how does git work").
- Topics outside Kyle's domain pattern -- the KB is heavy on R/infra/Quarto; pure JS algorithm questions probably aren't in it.
- Anything you can answer faster from the current code than from history.
