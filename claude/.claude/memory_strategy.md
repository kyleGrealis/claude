# Memory Strategy

Last updated: 2026-04-19

This document describes how memory works across Kyle's two AI contexts:
**Claude Code** (sessions on Pi5 and archMitters) and **NanoClaw/Andy** (Discord-based agent).
Both share the same underlying storage but are accessed differently.

---

## Overview

Three layers of persistent memory, in order of preference:

| Layer | Location | Best for |
|-------|----------|----------|
| **Curated memory files** | `~/nanoclaw/groups/discord_main/memory/*.md` | Durable facts, decisions, preferences, infra |
| **Conversation logs** | `~/nanoclaw/groups/discord_main/conversations/` | Full context from past sessions with Andy |
| **Compiler daily logs** | `~/dev/claude-memory-compiler/daily/YYYY-MM-DD.md` | "What did I work on last week?" cross-session recall |

---

## Layer 1 - Curated Memory Files (Andy's memory/)

These are Andy's primary recall source. Maintained by Andy, structured for fast lookup.

| File | Contents |
|------|----------|
| `infrastructure.md` | Machines, services, backup system, Tailscale IPs, SSH rules, Google Calendar |
| `family.md` | Alexa, Sofia, relationships, personal context |
| `llm-tooling.md` | Provider decisions, Ollama vs Anthropic, git rules, compiler workflow |
| `pending-todos.md` | Open items across sessions |

**When to write here**: Any durable fact - a decision that was made, a preference Kyle expressed, a piece of infra that was set up, a correction. If it would be annoying to re-explain next session, it goes here.

**Format rules**:
- ISO 8601 dates
- Backtick all technical terms (commands, paths, service names, env vars)
- Keep entries concise - this is a lookup table, not a narrative
- If a file hits 500 lines, split into a folder

---

## Layer 2 - Conversation Logs

Full session transcripts. `INDEX.md` has one-line summaries per file. Use for "what did Andy and I discuss about X" queries where the curated files don't have it.

`2026-04-08-to-2026-04-10-archmitters-history.md` is the pre-Pi5-migration archive.

---

## Layer 3 - Claude Memory Compiler

**Auto-pipeline** (runs without Kyle doing anything):

1. Every Claude Code session (on Pi5 or archMitters): `SessionStart`, `PreCompact`, `SessionEnd` hooks flush summaries to `~/dev/claude-memory-compiler/daily/YYYY-MM-DD.md`
2. After 6pm local, if today's log changed: `compile.py` auto-runs, writes synthesized articles to `~/Documents/obsidian/dev/AI-Knowledge-Base/`

**Manual triggers**:
```bash
compile-memory              # compile today's log
compile-memory --all        # recompile everything
compile-memory --dry-run    # preview without writing
compile-memory --file daily/2026-04-17.md  # specific file
```

**LLM**: Ollama Pro (`qwen3.5:397b-cloud`) on archMitters. Runs independently of NanoClaw (Anthropic-only). Staggered: Pi5 compiles at 23:00, archMitters at 23:15 to avoid Syncthing conflicts.

**Output**: `~/Documents/obsidian/dev/AI-Knowledge-Base/` - do NOT hand-edit. Compiler owns this folder.

---

## Claude Code vs NanoClaw Memory

| Aspect | Claude Code (on Pi5/archMitters) | NanoClaw/Andy (Discord) |
|--------|----------------------------------|-------------------------|
| Primary memory source | This file + project CLAUDE.md | `memory/*.md` files in group folder |
| Recall trigger | Context window at session start | Explicit grep/read during conversation |
| Persistence mechanism | Memory compiler hooks | Andy writes to memory/*.md directly |
| Conversation history | `~/.claude/projects/` (session JSONL) | `conversations/` folder |
| Shared infra knowledge | `~/.claude/CLAUDE.md` + `~/.claude/memory_strategy.md` | `memory/infrastructure.md` |

The two contexts don't share memory automatically. Kyle's NanoClaw preferences/decisions should be kept in `memory/*.md`. Claude Code-specific preferences should be kept in CLAUDE.md and skills.

---

## Recall Reflex

**For Claude Code sessions**: Check project CLAUDE.md first, then this file, then check if the relevant info is in a skill (`/personal-context`). If it's something Kyle would know from a past Andy session - ask Kyle or check `conversations/INDEX.md`.

**For Andy (NanoClaw)**: When Kyle references something from a prior session - grep `memory/` first, then `conversations/INDEX.md` to pick the right log, then grep across `conversations/`. Never ask Kyle to paste context that should already exist.

---

## Writing Good Memory Entries

**Do**:
- Write decisions with the "why": not just "NanoClaw uses Anthropic" but "NanoClaw uses Anthropic via OneCLI - Ollama-as-provider experiment ended 2026-04-15 after Ollama-poison incident"
- Capture the resolution, not just the problem
- Date-stamp durable facts
- Cross-reference: if infrastructure.md has the Google Calendar info, link to it from the daily note

**Don't**:
- Write ephemeral session context into persistent memory (what we're working on right now)
- Duplicate between memory files and Obsidian - pick one as the source of truth
- Leave TODO-style items in memory files (use `pending-todos.md` for those)

---

## Obsidian Vault Integration

Daily notes live at `~/Documents/obsidian/Daily Notes/YYYY/April/Month DD, YYYY.md`.

Andy fills **Shipped Today** and **Dev** sections at session wrap. Kyle fills Personal. Bridge to Tomorrow is Kyle's.

`~/Documents/obsidian/NanoClaw/` - Andy's self-documentation, mirrors key memory/*.md content for vault visibility.

`~/Documents/obsidian/Infrastructure/` - machine/service docs with `[[Pi5]]`, `[[archMitters]]` wikilinks.
