# Claude Code Discipline Pack

Three tiny [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills that stop the assistant from **confidently shipping broken changes**.

If you use Claude Code (or Claude in an agentic setup) on a real codebase, you've hit all three of these:

- It says **"Done ✅"** — but the edit was a silent no-op and nothing actually changed.
- It **guesses** a column name / env var / function signature instead of checking, and quietly breaks a query.
- It writes a patch against what the file **used to** look like — a renamed field, a moved function — and the plausible-looking change is wrong.

These aren't model-intelligence problems. They're **discipline** problems, and discipline is exactly what a skill can enforce. Each skill below is a short `SKILL.md` that auto-triggers on the right situation and forces a check *before* the mistake ships.

## What's inside

| Skill | What it enforces |
|-------|------------------|
| **doubt-driven-development** | When unsure about a name/path/behavior → grep or query the live system *before* writing code. "Sounds right" is not verified. |
| **source-driven-development** | Before editing or referencing a file → pre-flight grep the live source. The code that runs is the spec; docs and memory drift. |
| **verification-before-completion** | Before saying "done" → grep-count the change, syntax-check it, prove migrations with a query. *Written* ≠ *verified*. |

Battle-tested driving an AI-built production app day after day — these are the three checks that caught the most silent bugs.

## Install

Skills live in a `.claude/skills/` folder. Drop these in either location:

**Global (all projects):**
```bash
git clone https://github.com/SpikeboyIL/claude-code-discipline-pack.git
cp -R claude-code-discipline-pack/skills/* ~/.claude/skills/
```

**Single project:**
```bash
cp -R claude-code-discipline-pack/skills/* /path/to/project/.claude/skills/
```

That's it. Claude Code reads each skill's `description` and triggers it automatically when the situation matches — no config, no slash command required. You can also invoke one explicitly (e.g. `/verification-before-completion`).

## Why these three

They compose into one loop: **doubt → check the source → verify before done.** Front of the change, middle of the change, end of the change. Most "the AI broke it" moments are one of these three steps skipped.

## License

MIT — use them, fork them, ship them. See [LICENSE](LICENSE).

---

### Want the full playbook?

These skills came out of actually **building and shipping real projects with Claude Code** — not toy demos. If that's what you're after, two practical guides:

- **[Build & Launch With Claude Code — Ship a Real Website, From Blank Folder to Live](https://spikeboy3.gumroad.com/l/cprhfg?utm_source=github&utm_medium=readme&utm_campaign=discipline-pack)**
- **[Learn + Build with Claude — Master Claude, Then Ship a Real Website (2-book bundle)](https://spikeboy3.gumroad.com/l/szxbmy?utm_source=github&utm_medium=readme&utm_campaign=discipline-pack)**

Built by the team behind [promly.ai](https://promly.ai?utm_source=github&utm_medium=readme&utm_campaign=discipline-pack).
