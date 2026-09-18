# rules

**What it does:** Loads selectable "rule pack" markdown files for the current session, on top of the always-on CLAUDE.md rules, and lets you check or force-recheck what's currently in effect.

**When to use it:**
- At the start of a session that needs a specific rule set (e.g. a particular client's detection standards)
- Any time you want to see exactly what's active
- The moment you spot it breaking a rule — as a fast nudge, not a full report

**How to invoke:**
- `/rules` — lists available packs from `~/.claude/rule-packs/`
- `/rules <pack-name> [<pack-name> ...]` — loads one or more packs; re-running a name reloads it fresh from disk and shows you what changed
- `/rules status` — reports what's active: always-on CLAUDE.md files, plus any packs loaded this session, and notes precedence if more than one pack is loaded
- `/rules recheck` — fast self-audit: re-reads active rules, checks its own recent output against them, states plainly if it diverged, and corrects course

**Where rule packs live:** `~/.claude/rule-packs/*.md` — one flat folder, no sub-hierarchy. Edit a file directly to update it; the change takes effect the next time that pack is loaded or reloaded.

**Where the skill itself lives:** `~/.claude/skills/rules/SKILL.md` (personal scope)
