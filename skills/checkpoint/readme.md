# checkpoint

**What it does:** Curates the session's key decisions, current state, and open questions into a resumable markdown file — a deliberately better alternative to relying on Claude Code's autocompact summary when context is filling up.

**When to use it:** Manually, right before you expect auto-compaction (you can see the context window closing in in Antigravity). It never triggers itself — `disable-model-invocation: true`.

**How to invoke:**
- `/checkpoint` — writes to the default `docs/session-notes/` in the current project
- `/checkpoint <path>` — write somewhere else instead

**What happens:**
1. Checks whether this session already has a checkpoint file (matched by session ID) — updates it if so, starts a new one named `<date>_<slug>.md` if not
2. Shows the full proposed content and waits for your confirmation or edits before writing anything
3. Writes the file once confirmed
4. Suggests a short session-rename title (repo(s) + task) — you apply it yourself, this isn't automated

**Where it lives:** `~/.claude/skills/checkpoint/SKILL.md` (personal scope — works the same in any project)
