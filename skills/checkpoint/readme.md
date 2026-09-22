# checkpoint

**What it does:** Curates the session's key decisions, current state, and open questions into a resumable markdown file — a deliberately better alternative to relying on Claude Code's autocompact summary when context is filling up.

**When to use it:** Manually, right before you expect auto-compaction (you can see the context window closing in in Antigravity). It never triggers itself — `disable-model-invocation: true`.

**How to invoke:**
- `/checkpoint` — writes to the default `docs/session-notes/` in the current project
- `/checkpoint <path>` — write somewhere else instead

**What happens:**
1. Checks whether this session already has a checkpoint file (matched by session ID) — updates it if so, starts a new one named `<date>_<slug>.md` if not
2. Determines rule packs active and skills invoked this session from what it can see, asking only if genuinely unclear — recorded so the `sessions` skill can later filter/analyze by them
3. Shows the full proposed content and waits for your confirmation or edits before writing anything
4. Writes the file once confirmed, with a `Status: open` field (only `sessions close` ever changes this)
5. Suggests a short session-rename title (repo(s) + task) — refreshed every time you run it, including in the saved file on later checkpoints, so it stays current as the task evolves. You apply it yourself; there's no confirmed tool to do the rename itself
6. Advises compact-or-fresh-resume: a rough proxy (tool calls/files touched, first vs. repeat checkpoint), clearly labeled as approximate since Claude Code doesn't expose real context usage to a skill, plus a real recommendation and the exact command either way — `/compact` yourself to continue leaner, or open a new session and say `Read <file> and resume from there` for a genuinely clean slate

**Where it lives:** `~/.claude/skills/checkpoint/SKILL.md` (personal scope — works the same in any project)
