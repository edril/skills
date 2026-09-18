---
name: lessons
description: Project-scoped, git-tracked knowledge base of lessons learned — capture manually, harvest from checkpoints/issues, recall scoped by topic before falling back to git-log archaeology
argument-hint: 'new <topic> <short-name> <lesson> | recall [topic=<topic>] [keyword] | harvest [topic] | topics'
allowed-tools: Bash(git:*) Bash(date:*) Read Write Edit Glob Grep
disable-model-invocation: true
---

# Lessons

Arguments: `$ARGUMENTS`

Storage: `docs/lessons/*.md` inside the current project repo — git-tracked, commit these like any other file so the knowledge base travels with the project. One file per lesson.

Filename: `<date>_<topic>_<short-name>.md`, date from `!`date +%Y-%m-%d``.

Frontmatter template:
```
---
Topic: <topic>
Short name: <short-name>
Date: <date>
Repos:
  - repo: <name>
    commit: <sha-or-none>
Source: manual | harvested
---

## Lesson
<what was tried / what worked or didn't / why>
```

## No arguments — overview
Glob `docs/lessons/*.md`, group by `Topic`, and show each topic with a count. Point to `recall` and `harvest` for what to do next.

## `topics`
Same grouping as above but just the topic list with counts — nothing else. Use this before `recall topic=...` if you don't remember exact topic names.

## `new <topic> <short-name> <lesson text>`
1. Capture the relevant repo(s) + commit via git, same approach as `/issues new`: auto-detect from the current directory; if this session has touched more than one relevant repo, ask which one(s) this lesson applies to rather than assuming.
2. Write the file with `Source: manual`.
3. Confirm in one line — filename + topic. Keep this fast, no full playback; the point is capturing a lesson the moment it's learned without breaking stride.

## `recall [topic=<topic>] [keyword ...]`
1. Glob `docs/lessons/*.md`.
2. If `topic=<topic>` is given, filter to that `Topic` first — apply this scope before anything else, it's the whole point of tagging.
3. Within that scope (or across everything if no topic given), match any remaining keyword(s) against filename, Topic, and lesson body.
4. Show matches: short name, topic, date, one-line gist of the lesson.
5. If nothing found: say so plainly. Only then ask whether to fall back to a genuinely deeper search — git log / commit messages / PR history for the same keyword(s). Never do that automatically; it's the expensive path this whole skill exists to avoid defaulting to.

## `harvest [topic]`
1. Glob checkpoint files relevant to this project (`docs/session-notes/*.md`) and personal issue files that tag this repo (`~/.claude/issues/*.md`, filtered to matching repos).
2. Read them and identify candidate lessons: decisions with a stated rationale, resolved issues with their actual fix, "next step" items that turned out non-trivial or recurred, problems that showed up more than once across entries.
3. If `topic` was given, surface only candidates plausibly belonging to it; otherwise surface everything found and propose a topic per candidate.
4. **Playback before writing** — list every candidate with its proposed topic/short-name/lesson text and let me confirm, drop, or edit each one individually. Write nothing until confirmed.
5. Write confirmed ones as normal lesson files, `Source: harvested`.
