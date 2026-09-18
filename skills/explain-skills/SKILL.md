---
name: explain-skills
description: Explains what your custom skills and commands do and when to use them, selectively by name. For the full up-to-date list of everything currently loaded, points to the built-in /skills command instead.
argument-hint: '[skill-or-command-name ...] (optional; omit for guidance, or "all" for every custom one)'
allowed-tools: Glob Read
---

# Explain Skills & Commands

Arguments: `$ARGUMENTS`

## No arguments
Don't scan anything. Tell me to run the built-in `/skills` command for the full, always-current list of everything loaded (name, token cost, description, with sort/hide controls). Mention this skill is for going deeper on specific ones once I know which I want — e.g. `/explain-skills sigma-to-kql commit` or `/explain-skills all`.

## With arguments
Treat `$ARGUMENTS` as one or more space-separated names, or the literal word `all`.

Search these locations:
- Project skills: `${CLAUDE_PROJECT_DIR}/.claude/skills/<name>/SKILL.md`
- Project commands: `${CLAUDE_PROJECT_DIR}/.claude/commands/<name>.md` (and namespaced subdirs)
- Personal skills: `~/.claude/skills/<name>/SKILL.md`
- Personal commands: `~/.claude/commands/<name>.md`

If a named one isn't found in any location, say so plainly — don't describe it from memory.

For `all`: Glob every `SKILL.md` under `${CLAUDE_PROJECT_DIR}/.claude/skills/*/` and `~/.claude/skills/*/`, plus every `.md` under both `commands/` directories (recursively, for namespaced ones).

For each match, **Read the full file** — not just the description already cached in your context — and report:
- **Name** and how to invoke it (`/name`, with `argument-hint` if any)
- **Scope** — project or personal
- **What it does** — in plain terms, from the body
- **When to use it** — trigger conditions; note if `disable-model-invocation: true` (manual only) vs. Claude-invocable too
- **Anything notable** — `context: fork` (runs as a subagent), pre-approved `allowed-tools`, required arguments

Keep each entry tight. One heading per name if several were given, so they're easy to scan separately.
