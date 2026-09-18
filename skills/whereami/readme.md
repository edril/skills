# whereami

**What it does:** Answers "where am I in this?" at two zoom levels — replaces the separate progress updater / summarize progress / brancher ideas with one read-only skill.

- **Small picture** — since your last actual instruction, what's been done, whether it drifted or assumed things you didn't ask for, and whether it still matches what you asked. Ends by handing you the choice: continue, or pause and realign.
- **Big picture** — the whole session: original goal(s) (can be more than one), what's complete/in-progress/parked/abandoned. Grounds itself against a `/checkpoint` file for this session if one exists, rather than only re-deriving from the live conversation. This also covers what "brancher" was for — it's pure visibility into how things have branched, no separate branch-marking step needed.

**When to use it:** Any time you've lost your place — especially after the console has run ahead with low supervision and you're not sure what happened since you last typed something.

**How to invoke:**
- `/whereami` or `/whereami small` — small picture (default)
- `/whereami big` — big picture
- `/whereami both` — both, small first

**Where it lives:** `~/.claude/skills/whereami/SKILL.md` (personal scope)
