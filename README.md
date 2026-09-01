# seed

A minimal Claude Code project-context template. Clone it as the starting point for a new project so Claude has full, durable context from message one, instead of re-explaining the project every session.

## What's in here

- **CLAUDE.md** — the entry point, auto-loaded by Claude Code at the start of every session. Points at the files below and explains how to keep them current.
- **PLAN.md** — current state: what's done, in progress, next. This is meant to change often.
- **STANDARDS.md** — project-specific conventions (style, testing, things to avoid). Read before writing code.
- **DECISIONS.md** — a running log of non-obvious technical/architectural calls and why they were made, so past reasoning doesn't get silently re-litigated.
- **.claude/hooks/session-start.sh** — a `SessionStart` hook that injects PLAN.md's content into context automatically at the start of every session, as a forcing function on top of CLAUDE.md pointing at it.
- **.claude/commands/** — three slash commands:
  - `/refresh` — reload PLAN.md/STANDARDS.md/DECISIONS.md and summarize current state. Good after a long session or before compaction.
  - `/plan-update` — has Claude rewrite PLAN.md to match what's actually true right now.
  - `/decide "..."` — logs a decision to DECISIONS.md in a consistent format.

## Using this template

```
git clone git@github.com:LumosDhia/seed.git my-new-project
cd my-new-project
rm -rf .git && git init
```

Then fill in the `<!-- Project-specific notes -->` section of CLAUDE.md, and replace the placeholder content in PLAN.md / STANDARDS.md / DECISIONS.md as the project takes shape. Start a Claude Code session as normal — CLAUDE.md loads automatically, the SessionStart hook surfaces PLAN.md, and the slash commands are ready to use immediately.

## Why this shape

Three small, purpose-specific files instead of one sprawling CLAUDE.md keeps each one easy to keep accurate: PLAN.md changes daily, STANDARDS.md changes rarely, DECISIONS.md only ever grows. CLAUDE.md itself stays a thin pointer — cheap to load every session — with the heavier files pulled in only when relevant (STANDARDS.md before coding, DECISIONS.md when the task touches something already decided).

## Extending it

This ships with one hook (`SessionStart`) because its context-injection contract is the one this template relies on and knows works. `PreToolUse`/`PostToolUse`/`Stop` hooks are natural next additions (e.g. reminding about STANDARDS.md before an edit, or nudging a `/plan-update` when a session ends without one) — add them to `.claude/settings.json` as the project's actual needs become clear, rather than shipping speculative ones here.
