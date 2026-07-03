---
name: doubt-driven-development
description: >-
  Use whenever there is uncertainty about a column name, function name, env var,
  route path, or behavioral detail — before writing code that depends on it.
  Doubt is a signal to investigate, not a blocker to work around. Do NOT guess
  when a grep would resolve it in five seconds.
---

# Doubt-Driven Development

> When in doubt — grep, don't guess. Doubt is data, not an obstacle.

## When this applies

- Unsure of the exact column name in a table you're querying.
- Not certain whether an env var is `API_TOKEN` or `API_KEY`.
- Fuzzy on whether a function exists or what it returns.
- Uncertain whether a pattern is the current way or the old way.
- "I think it's X but let me check" — that thought IS the signal.

## What to do

1. **Name the doubt** — surface it explicitly. Don't bury uncertainty inside a
   confident-sounding statement.

2. **Grep first** —
   `grep -rn "suspected_name" src/ 2>/dev/null | grep -v node_modules`.
   Three seconds. Resolves definitively.

3. **If grep is inconclusive** — query the live system directly:
   - Column exists? Ask the database:
     `SELECT column_name FROM information_schema.columns WHERE table_name = 'your_table'`.
   - Env var in use? Grep the config/deploy files, not your memory.
   - Function signature? `grep -n "function_name" src/` and read the definition.

4. **Note the finding** — if the investigation surfaces real drift (the docs say
   one thing, the code does another), flag it. One doubt resolved now prevents a
   silent bug later.

## Anti-pattern

**Never guess and proceed.** A wrong column name silently breaks a query. A wrong
env var silently disables a job — at 3am, with no error, no alert, and no audit
trail. The cost of a wrong guess always exceeds the cost of a grep.

## Examples

**Good:** "Not sure if it's `user_token` or `ACCESS_TOKEN` →
`grep -rn 'user_token\|ACCESS_TOKEN' src/` → found `ACCESS_TOKEN` → code uses the exact name."

**Near-miss (never do this):** "I'll use `session_id` — that sounds right."
Sounds right is not verified. Verified is the only bar.
