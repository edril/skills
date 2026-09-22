# sessions

**What it does:** Queries and analyzes the checkpoint corpus (`docs/session-notes/*.md`) — the files `/checkpoint` writes. Read-only except for `close`. Doesn't write checkpoints itself.

**When to use it:** Any time you want to work with past sessions rather than just the current one — finding one, comparing two, or spotting patterns across many.

**How to invoke:**
- `/sessions` — quick list of open sessions
- `/sessions list [status=open|closed|all] [repo=<name>] [pr=<num>]` — filter
- `/sessions show <name>` — full content of one
- `/sessions summary <name>` — tight synopsis instead of the full file
- `/sessions diff <nameA> <nameB>` — substantive comparison (state, decisions, rules active, skills invoked, repos), not a line diff
- `/sessions close <name>` — marks it closed
- `/sessions analyze <question>` — free-form pattern analysis across sessions, e.g. "which rule packs pair well with which skills." Offers to also pull in `lessons` alongside session notes. Explicitly qualitative — a read across available files, not real statistics, and it says so if the corpus is too thin to conclude anything.
- `/sessions analyze skills` — shortcut: cross-references every skill actually installed against how often it shows up in "Skills invoked" across sessions, grouped into frequently used / rarely used / never appearing — a way to spot skills that might be stale or unused, with the same caveat that it only reflects sessions that got checkpointed

**Depends on:** `/checkpoint` now recording `Status`, `Rule packs active`, and `Skills invoked` — sessions checkpointed before that change won't have those fields to filter/analyze on.

**Where it lives:** `~/.claude/skills/sessions/SKILL.md` (personal scope)
