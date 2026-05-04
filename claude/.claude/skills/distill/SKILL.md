---
name: distill
description: At the end of a session involving a non-trivial problem-solving arc, evaluate whether to crystallize the lesson into a skill, a memory entry, or a daily-log addition. Use when (a) we hit real dead ends and resolved them, (b) Kyle says "let's remember this" / "save this", or (c) you recognize a pattern that will recur with a clear future trigger. Don't invoke for routine tasks.
---

# Distill -- Capture Reusable Lessons

Goal: turn this session's hard-won insight into something a future session benefits from -- without creating skill spam or duplicate memory.

## Decide before writing

Walk these in order. The first "yes" picks the home.

1. **Does it already fit an existing skill?** -> Append a section there. Do **not** create a new skill. An R package gotcha goes into `r-package`; a Cloudflare deploy quirk into the relevant CF skill.
2. **Is it a workflow with a clear future trigger?** (A signal in a future session will recognizably match: a file pattern, an error message, a tool combination.) -> Draft a NEW skill. Trigger description must be specific. "Use when editing Cloudflare Worker bindings" beats "use when working on web stuff."
3. **Is it a fact, decision, or one-time discovery?** -> Memory entry, not a skill. Use `auto memory`: append to MEMORY.md + write a typed file (feedback / project / reference / user). For dated incidents, the daily log at `~/dev/claude-memory-compiler/daily/<YYYY-MM-DD>.md` is also valid.
4. **Is it general programming knowledge or a one-off?** -> Skip. Don't capture.

## Skill quality bar

A new skill earns its slot only if **all three** hold:

- The trigger description names specific, recognizable signals (file paths, error strings, tool names) -- not vague topics.
- The pattern recurs. One past incident does not justify a skill.
- It tells you *what to do*, not just *what happened*. ("When you see X, do Y because Z" -- not "we had a problem with X once.")

If any fails, downgrade to memory.

## Format check before saving

- Frontmatter `description` IS the trigger -- write it for matching, not for humans browsing.
- Body: lead with the rule, follow with rationale, close with edge cases or counter-examples.
- SKILL.md should be readable in 30 seconds. If longer, split or move detail into linked files.

## Offer, don't impose

End-of-session: propose to Kyle, "Want me to distill X into [skill / memory / daily log]?" Show the proposed home and a 2-line preview. Let him pick. Don't auto-write.

## Anti-patterns to avoid

- "Skill per incident." A dozen narrow skills become dead weight in every session's prompt.
- Re-capturing what an existing skill already covers.
- Writing skills with vague triggers ("use when working on backend stuff") -- they false-positive.
- Saving general programming knowledge ("how git rebase works") -- that's training data, not a personal skill.
