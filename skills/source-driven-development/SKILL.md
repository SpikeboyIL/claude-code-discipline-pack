---
name: source-driven-development
description: >-
  Use before editing any existing file and before writing code that touches a
  known path. The live source file is ground truth — not docs, not a prior plan,
  not memory. Do NOT skip the pre-flight grep even when the file structure
  "feels" known.
---

# Source-Driven Development

> The code that runs is the spec. Everything else is a hypothesis.

## When this applies

- Before writing a patch on a file you have not read this session.
- Before referencing any column name, function signature, CSS class, or env var.
- Before trusting a design doc, README, or your own memory about how a file looks.

## What to do

1. **Pre-flight `grep -n`** — before writing anything that references a specific
   identifier, run `grep -n "identifier" path/to/file`. Confirm existence, exact
   spelling, and surrounding context. No assumptions from memory.

2. **Read the target block** — open the actual function or section being modified.
   Do not write a patch against a stale mental model of it.

3. **Repo-wide search** — `grep -rn "term" src/ 2>/dev/null | grep -v node_modules`
   when the file's location is uncertain.

4. **Never treat as authority** a prior plan, a design doc, or recalled structure
   for what a live file contains. Docs drift. Memory drifts. The file on disk does
   not — read it.

5. **The pre-flight grep is the cheapest step you will ever run.** Drift caught at
   grep time costs nothing; drift shipped into a change costs a debugging session.

## Why this matters

The single most common source of AI-assisted bugs is a change written against what
the file *used to* look like — a renamed column, a moved function, a class that no
longer exists. The assistant is confident, the patch is plausible, and it is wrong.
A three-second grep against the live file is the only defense, and there is no
backup for it.

## Examples

**Good:** Before editing the auth middleware — `grep -n 'requireUser' src/middleware/` →
confirms the exact export name and signature → patch references live code, not memory.

**Near-miss (never do this):** "I'll base this on how the other middleware is
structured" — without grepping it first, the patch is written against memory.
Memory drifts. The drift ships.
