# issues

**What it does:** Tracks problems you spot mid-session but can't fix right now — one markdown file per issue, tagged with the repo(s)/commit(s)/PR(s) it relates to (captures all of them if it spans more than one repo), timestamped, filterable, and resolvable.

**When to use it:** The moment you notice something worth coming back to later. Designed to be low-friction — `new` doesn't do a full playback-and-confirm like `/checkpoint`, it just captures and confirms in one line.

**How to invoke:**
- `/issues` — quick list of currently open issues
- `/issues new <short-name> <description...>` — create one; auto-captures repo/commit (and PR if `gh` finds one) for every repo relevant to it, asking which repo(s) if more than one has been touched this session
- `/issues list [repo=<name>] [status=open|resolved|all] [commit=<sha>] [pr=<num>]` — filter
- `/issues show <short-name>` — full content of one issue
- `/issues resolve <short-name>` — marks it resolved with a date

**Where issues live:** `~/.claude/issues/*.md` — one flat personal directory, so you can query across every repo you work in from one place. Files are plain markdown, readable/editable directly on disk.

**Where the skill itself lives:** `~/.claude/skills/issues/SKILL.md` (personal scope)
