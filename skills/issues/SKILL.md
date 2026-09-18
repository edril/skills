---
name: issues
description: Tracks problems you spot mid-session but can't fix yet — one file per issue, tagged with repo/commit/PR (multiple if it spans them), timestamped, filterable, resolvable
argument-hint: 'new <short-name> <description> | list [repo=<name>] [status=open|resolved|all] | show <short-name> | resolve <short-name>'
allowed-tools: Bash(git:*) Bash(gh:*) Bash(date:*) Read Write Edit Glob Grep
disable-model-invocation: true
---

# Issues Tracker

Arguments: `$ARGUMENTS`

Storage: `~/.claude/issues/` — one flat directory, one markdown file per issue. Personal, not project-scoped, so issues are queryable across every repo you work in.

Filename: `<date>_<time>_<short-name>.md` (e.g. `2026-09-18_1442_flaky-kql-timestamp.md`) — date/time from `!`date +%Y-%m-%d_%H%M``, short-name kebab-cased from what's given.

## No arguments — quick list
Glob `~/.claude/issues/*.md`, read just the frontmatter of each, and list open ones only: short name, date, repo(s). Say how many resolved ones are hidden. Mention `/issues list status=all` for everything.

## `new <short-name> <description...>`
1. Determine every repo relevant to this issue — not just the current directory. If this session has touched more than one repo, ask me which one(s) this issue applies to rather than assuming just the cwd.
2. For **each** relevant repo, capture, silently skipping anything unavailable rather than failing the whole thing:
   - Repo name: `git remote get-url origin` (fall back to folder name if no remote)
   - Commit: `git rev-parse HEAD`
   - PR: `gh pr view --json number,url` if `gh` is available and the branch has an open PR; otherwise omit
3. Write the file:

```
---
Short name: <short-name>
Date: <date>
Time: <time>
Status: open
Session ID: ${CLAUDE_SESSION_ID}
Repos:
  - repo: <name>
    commit: <sha>
    pr: <url-or-none>
  - repo: <name2>
    commit: <sha2>
    pr: <url-or-none>
---

## Problem
<description as given, lightly cleaned up — don't editorialize or expand on it>
```

4. Confirm creation in one line: filename + which repo(s)/commit(s) got attached. Don't do a full playback-and-wait like `/checkpoint` — this needs to be fast, since the point is capturing something mid-flow without breaking stride.

## `list [repo=<name>] [status=open|resolved|all] [commit=<sha>] [pr=<num>]`
Default `status=open` if not given. Glob all issue files, filter by whatever key=value pairs were given (matching inside the `Repos:` block for repo/commit/pr), and list matches: short name, date, status, repo(s).

## `show <short-name>`
Match against filenames (fuzzy on the short-name part is fine). Show the full file content. If more than one matches, list them and ask which.

## `resolve <short-name>`
Match as in `show`. Edit that file: `Status: open` → `Status: resolved`, and add a `Resolved: <date>` line under it. Confirm briefly.
