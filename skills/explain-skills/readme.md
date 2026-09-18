# explain-skills

**What it does:** Reads the actual SKILL.md / command files you've built — project and personal — and explains what each one does and when to use it. A deeper, selective companion to the built-in `/skills` browser, not a replacement for it.

**When to use it:** Once you already know roughly which skill(s) you want explained, or want a full read-through of everything you've built so far.

**How to invoke:**
- `/explain-skills` — no arguments: points you to the built-in `/skills` command for a quick browse instead
- `/explain-skills <name> [<name> ...]` — deep explanation of specific skills/commands by name
- `/explain-skills all` — explains every custom skill/command found

**What happens:** Globs and reads the real files (not just the cached description already sitting in context) from both project (`.claude/skills/`, `.claude/commands/`) and personal (`~/.claude/skills/`, `~/.claude/commands/`) locations, and reports name, scope, what it does, when to use it, and anything notable (e.g. `disable-model-invocation`, `context: fork`, pre-approved tools).

**Where it lives:** `~/.claude/skills/explain-skills/SKILL.md` (personal scope)
