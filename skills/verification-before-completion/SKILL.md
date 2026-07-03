---
name: verification-before-completion
description: >-
  Use before declaring any task done, complete, or ready to ship — on any
  string-replace, file edit, code change, or migration. Do NOT skip because the
  change "seems small"; small changes are where unverified work slips through.
---

# Verification Before Completion

> Done means verified, not just written.

## When this applies

Before saying "done", "complete", "ready", or handing the work back — on any
file edit, string replacement, code addition, or database migration.

## What to do

1. **After every string-replace edit** — run `grep -c "changed_identifier" path/to/file`.
   Verify the count matches what you expected. Count = 0 means a silent no-op; that
   is a real bug, not a success.

2. **Two lockstep strings** — `grep -c` returns 1 when both targets sit on the same
   line. Use separate `grep` calls when verifying a paired change (e.g. a constant
   and the label that must stay in sync with it).

3. **Syntax-check what you touched** — run the language's check before declaring done
   (`node --check file.js`, `python -m py_compile file.py`, `tsc --noEmit`, a linter,
   or the build). The editor accepting the change is not verification.

4. **Smoke grep** — grep for the key identifier you just added. Confirm it appears
   exactly where expected, exactly N times.

5. **Migrations** — follow the migration with a query that proves the new state:
   a row count, a permission check, or a SELECT on the new columns. A migration
   command returning "success" is not proof the schema is what you intended.

## Examples

**Good:** edit applied → `grep -c 'ACCESS_TOKEN' src/auth/token.js` → 3 ✅ →
`node --check src/auth/token.js` → OK → done.

**Near-miss (never do this):** "I've added the handler. Done." — no grep run, no
syntax check. The tool accepting the change is not verification.
