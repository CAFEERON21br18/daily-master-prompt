---
name: create
description: Use when the user wants a daily master prompt, a recurring or unattended work-session prompt, or a "pick up where we left off every day" routine for a project, or wants to change or add an addendum to an existing DAILY_PROMPT.md.
argument-hint: "[project folder] [what the project is and what it should reach]"
---

# Create a Daily Master Prompt

## Overview

A daily master prompt is one self-contained file, `DAILY_PROMPT.md`, that any fresh session can execute to pick up a project, do a day of work, and leave it ready for the next day. The project's memory lives in five state files next to it, never in the conversation. After the first run the user only types:

```
Read <folder>/DAILY_PROMPT.md and execute it.
```

Input from the user: `$ARGUMENTS` (a folder and/or an explanation of the project; either may be missing).

## What exists when you finish

All in the project folder (`<folder>`), built from the templates in `${CLAUDE_SKILL_DIR}/templates/`:

| File | Holds | Changes |
|---|---|---|
| `DAILY_PROMPT.md` | Mission, milestone, rules, context map, session procedure | Only when the user asks |
| `BACKLOG.md` | Ordered work items with IDs and status | Every run |
| `DEVLOG.md` | Append-only dated log of each run | Every run |
| `HANDOFF.md` | Current state, next 3 actions, warnings | Rewritten every run |
| `QUESTIONS.md` | Questions for the user, with a recommendation each | Runs ask, user answers |
| `APPROVALS.md` | Risky or irreversible actions waiting for yes/no | Runs request, user decides |

Progress goes in the state files. `DAILY_PROMPT.md` holds only what stays true for weeks.

## Steps

1. **Find the folder.** Use the folder in the arguments. Otherwise use the folder holding the project's spec or code; if several fit, ask.
2. **Gather context by reading, not asking.** The user's explanation; `README*`, `CLAUDE.md`, `AGENTS.md`; specs (GDD, PRD, roadmap, `docs/`); `git log --oneline -30` and `git status`; a 2-3 level file tree; manifests and scripts that reveal build, test, lint and run commands; connected MCP servers the work needs; TODO/FIXME notes; any existing state files. Separate what is done from what is not.
3. **Ask only for what the files don't say**, in one AskUserQuestion call (max 4 questions), from this list: current milestone and its definition of done; how a finished item is verified; hard rules (never publish/deploy, ask before new dependencies, branch and push policy); whether runs are attended, unattended, or both. If no one answers, take the safest reading (unattended, no push, no publish) and record each assumption in `QUESTIONS.md` with its "Assumed until answered" line.
4. **Write `DAILY_PROMPT.md`** from `DAILY_PROMPT.template.md`. Every section names this project's real paths, commands, tools and milestone. In the daily line, write the folder relative to the repository root (where the user opens Claude), not an absolute path; when the folder is the root itself, the line is `Read DAILY_PROMPT.md and execute it.` If running or testing the project could touch real data (user files, live accounts, production, a published place), add a ground rule and a verification method that use a temporary or test copy instead.
5. **Create the state files** from their templates. If one already exists, merge into it; never overwrite or reorder the user's history. Seed `BACKLOG.md` from the milestone, spec gaps, TODOs and the user's goals, ordered so item 1 can start today and each item fits in one session, each with a concrete "Done when". Seed `HANDOFF.md` with the starting state. A template whose example entry doesn't apply yet keeps its header and format lines, with `None yet.` in place of the entry.
   Check: `grep -n "{{" <folder>/*.md` returns nothing.
6. **Commit** the new files if the folder is in a git repo (`chore: add daily master prompt`), following the repo's commit rules.
7. **First run.** Unless the user said not to, execute `DAILY_PROMPT.md` now, exactly as a fresh session would. This is the prompt's test: when a step was ambiguous, failed, or needed knowledge that isn't in the files, fix the prompt and note the fix in `DEVLOG.md`.
8. **Hand off.** Tell the user what was created, what the first run did, and give the daily line in a code block:
   ```
   Read <folder>/DAILY_PROMPT.md and execute it.
   ```
   Mention `/daily-master-prompt:run <folder>` as the same thing, and that the line also works as the prompt of a scheduled task.

## Updating an existing prompt

When `DAILY_PROMPT.md` already exists, read it and every state file first. Apply the change in place, or under `## Addenda` as a dated entry when the user asks for an addendum. Keep all state. Log the change in `DEVLOG.md`.

## Common mistakes

| Mistake | Result | Do instead |
|---|---|---|
| Generic prompt ("work on the project") | Each run rediscovers the project | Real paths, commands, milestone, done-definition |
| Progress written into `DAILY_PROMPT.md` | Prompt goes stale and contradicts state files | Progress lives in `BACKLOG`/`HANDOFF`/`DEVLOG` |
| No verification step | "Done" items that don't work | A concrete check per item (tests, build, playtest, lint) |
| Runs stop at the first blocker | Unattended days produce nothing | Log in `QUESTIONS`/`APPROVALS`, continue with the next item |
| Backlog items too big | Half-finished work every day | Split until one item fits one session |
| Overwriting existing state files | User loses history | Merge |
