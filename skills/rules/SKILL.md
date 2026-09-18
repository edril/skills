---
name: rules
description: Loads selectable rule packs from ~/.claude/rule-packs/ for the current session, on top of always-on CLAUDE.md rules; also reports what's active and can self-audit recent output against them
argument-hint: '[pack-name ...] | status | recheck (omit to browse available packs)'
allowed-tools: Glob Read
disable-model-invocation: true
---

# Rule Selector

Arguments: `$ARGUMENTS`

Rule packs live at `~/.claude/rule-packs/*.md` — one flat folder, no sub-hierarchy. Edit any file there directly to update its content; changes take effect the next time you load or reload that pack with this skill. They sit alongside, not instead of, CLAUDE.md — CLAUDE.md is always-on and outside this skill's scope entirely.

## No arguments — browse
Glob `~/.claude/rule-packs/*.md`. For each, read enough to show its filename and first non-empty line as a one-line description — don't dump full content. List them and ask which to load, e.g. `/rules detection-client-a` or several together: `/rules detection-client-a sigma-conversion`.

## Argument is "status"
Report what's currently in effect, in two parts:

**Always-on (CLAUDE.md):**
- Check for and list whichever of these exist: enterprise/managed CLAUDE.md, `~/.claude/CLAUDE.md` (personal), `CLAUDE.md` at the project root, and any nested-directory `CLAUDE.md` relevant to where you're working.
- State plainly these load automatically every session and aren't optional — this skill doesn't control them.

**Selectable (rule packs):**
- Look back through this session for every prior `/rules <name>` invocation and list which pack(s) are currently loaded.
- If more than one is loaded, note explicitly that a more specific pack takes precedence over a more general one where they conflict — flag any conflict you can see rather than silently picking one.
- If none loaded this session, say so.

Close by restating where packs live (`~/.claude/rule-packs/`) and how to update them (edit the file directly, then re-run `/rules <name>` to reload).

## Argument is "recheck" — fast self-audit
This is triggered because I've spotted you breaking a rule. Don't produce a status report. Instead:

1. Re-surface, briefly, which rules are actually in force right now: CLAUDE.md (always-on) plus any rule pack(s) loaded this session. Don't skip this step — the point is a deliberate re-read, not trusting what you think you remember.
2. Look at your own most recent output/actions in this session and check them against those rules.
3. Say plainly, in one or two lines, where you diverged — don't soften it or explain it away.
4. Correct course immediately: restate or redo the affected part correctly, and continue applying the rule properly for the rest of the session.

Keep the whole response short. If nothing in your recent output actually violates anything you can find, say that directly rather than manufacturing a violation to report.

## Argument is one or more pack names
For each name, look for `~/.claude/rule-packs/<name>.md`. If missing, say so plainly — don't invent one.

If this pack was already loaded earlier in this session (its content is already visible above in this conversation), **read the file again now** and compare against what's already in context:
- **Unchanged** → say so briefly, don't re-print it.
- **Changed** → show what's different in short form, then treat the new version as authoritative going forward.

If loading for the first time this session, read and display the full content, and confirm it's now active.

When multiple packs are loaded together and their instructions conflict, the more specific one wins — say which you treated as more specific and why if it isn't obvious.
