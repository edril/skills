# Claude Code Skills

Personal Claude Code skills (`~/.claude/skills/`) built for this workflow, plus the project-scoped knowledge base one of them manages.

## Built

| Skill | Invoke | Scope | Purpose |
|---|---|---|---|
| **checkpoint** | `/checkpoint [path]` | Personal | Pre-compaction session state capture — curates decisions/state/open questions into a resumable markdown file, playback before write, suggests a session rename |
| **explain-skills** | `/explain-skills [names\|all]` | Personal | Selective deep explanation of custom skills/commands by reading their actual files; points to built-in `/skills` for a quick browse |
| **rules** | `/rules [pack-name\|status\|recheck]` | Personal (packs) | Loads selectable rule packs on top of always-on CLAUDE.md; reports what's active; fast self-audit nudge when a rule's been broken |
| **issues** | `/issues [new\|list\|show\|resolve]` | Personal | Tracks problems spotted mid-session but not yet fixed — one file per issue, tagged by repo/commit/PR (multiple if it spans them), resolvable |
| **whereami** | `/whereami [small\|big\|both]` | Personal | Where things stand — small picture (has recent work drifted from the last request) or big picture (goals, done/in-progress/parked/abandoned), grounded against a checkpoint file if one exists |
| **adversary-check** | `/adversary-check [dimension...]` | Personal | Self-audits recent output — matches request, consistent with what was agreed, truthful/non-simulated, current, rule-compliant, genuinely good quality. Pass/Concern/Fail, suggested fixes only |
| **lessons** | `/lessons [new\|recall\|harvest\|topics]` | **Project** (`docs/lessons/`, git-tracked) — skill itself is personal | Curated, topic-scoped knowledge base of lessons learned; fast recall before falling back to a git-log dig; harvest pulls candidates from checkpoints/issues for confirmation |

Each has its own `README.md` alongside its `SKILL.md` in `~/.claude/skills/<name>/`.

## Scope, at a glance
- **Personal, cross-project:** checkpoint, explain-skills, rules, issues, whereami, adversary-check
- **Project-scoped, git-tracked:** the `lessons` knowledge base (`docs/lessons/` in each repo) — deliberately different, since lessons belong to the project they're learned on

## To design

| Skill | Notes |
|---|---|
| usecase builder / tracker / tester | Not yet scoped — up next |
