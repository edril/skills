# sessions

**What it does:** Queries and analyzes the checkpoint corpus (`docs/session-notes/*.md`) — the files `/checkpoint` writes. Never writes checkpoints itself. Edits session files only for `close` and `index` (frontmatter only, after you confirm), and writes one derived file, `INDEX.md`.

**When to use it:** Any time you want to work with past sessions rather than just the current one — finding one, comparing two, or spotting patterns across many.

**How to invoke:**
- `/sessions` — quick list of open sessions
- `/sessions list [key=value ...]` — look sessions up by any recorded metadata: `status`, `repo`, `branch`, `commit`, `pr`, `file`, `date` (single day or range like `2026-09-01..2026-09-30`), `topic`, `skill`, `rulepack`, `issue`, `resumed-from`. Filters combine and match partially (SHA prefix, path fragment). Only defaults to open sessions when you give no filters at all
- `/sessions show <name>` — full content of one
- `/sessions summary <name>` — tight synopsis instead of the full file
- `/sessions diff <nameA> <nameB>` — substantive comparison (state, decisions, rules active, skills invoked, repos), not a line diff
- `/sessions close <name>` — marks it closed
- `/sessions index` — builds a library view of every session, however messy: one `INDEX.md` row per session with dates, repos/branches/commits/PRs, files, topic, issues raised, judged status, summary, outstanding items, and links (resumed-from / duplicates / related). Missing metadata on old sessions is filled in where it can be: dates and issues exactly, repos/files as written in the file, commits/PRs recovered from git by date window and clearly marked `(inferred)`, rule packs/skills left as `unknown`. Proposes everything and waits for your confirmation, then writes `INDEX.md` and adds missing fields to the originals (frontmatter only, body untouched). Derived, so safe to rerun any time.
- `/sessions outstanding` — what's still open across sessions: sessions are grouped into threads, only the latest in each thread counts, and status is judged from content rather than the `Status` field, so forgetting to close a session doesn't matter. Suggests rerunning `index` if it looks stale.
- `/sessions analyze <question>` — free-form pattern analysis across sessions, e.g. "which rule packs pair well with which skills." Offers to also pull in `lessons` alongside session notes. Explicitly qualitative — a read across available files, not real statistics, and it says so if the corpus is too thin to conclude anything.
- `/sessions analyze skills` — shortcut: cross-references every skill actually installed against how often it shows up in "Skills invoked" across sessions, grouped into frequently used / rarely used / never appearing — a way to spot skills that might be stale or unused, with the same caveat that it only reflects sessions that got checkpointed

**Depends on:** `/checkpoint` recording the metadata fields (dates, repos/branches/commits/PRs, topic, resumed-from, issues, rule packs, skills). Sessions checkpointed before that change won't have them; run `/sessions index` to add them retrospectively.

**Where it lives:** `~/.claude/skills/sessions/SKILL.md` (personal scope)
