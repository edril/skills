# whereami

**What it does:** Answers "where am I in this?" at two zoom levels — replaces the separate progress updater / summarize progress / brancher ideas with one read-only skill. Defaults to the big picture; can also save its report to disk.

- **Big picture (default)** — the whole session: original goal(s) (can be more than one), what's complete/in-progress/parked/abandoned. Grounds itself against a `/checkpoint` file for this session if one exists.
- **Small picture** — since your last actual instruction, what's been done, whether it drifted or assumed things you didn't ask for, and whether it still matches what you asked. Ends by handing you the choice: continue, or pause and realign.

**When to use it:** Any time you've lost your place — especially after the console has run ahead with low supervision and you're not sure what happened since you last typed something.

**How to invoke:**
- `/whereami` — big picture (default)
- `/whereami small` — small picture
- `/whereami both` — both, small first
- Add `save` or `save=<path>` to any of the above to also write the report to a markdown file — e.g. `/whereami big save`. With bare `save`, it proposes a default path (`docs/session-notes/whereami_<date>_<time>.md`) and asks you to confirm or override before writing.

**Where it lives:** `~/.claude/skills/whereami/SKILL.md` (personal scope)
