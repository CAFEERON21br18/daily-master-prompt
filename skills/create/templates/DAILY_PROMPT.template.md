# Daily Master Prompt: {{PROJECT_NAME}}

A fresh session runs this file with:

```
Read {{FOLDER}}/DAILY_PROMPT.md and execute it.
```

Everything this session needs is in this file and the state files it names. Do not rely on memory of earlier conversations. Paths below are relative to the repository root. Get today's date from the system (`date +%Y-%m-%d`) and use it for every dated entry.

## 0. Mission

{{One paragraph: what the project is, who it is for, what it must achieve.}}

- **Current milestone:** {{milestone name}}
- **Done means:** {{observable conditions, e.g. "all P1 items done, full test suite green, user run-through checklist in HANDOFF.md written"}}
- **When done is reached:** put `{{DONE_SIGNAL, e.g. MILESTONE READY}}` as the first line of `HANDOFF.md`, say it in the end-of-session report, and only take P2 items afterwards.

## 1. Ground rules

- {{rule from the user or CLAUDE.md, e.g. "Never publish or deploy; the user does that."}}
- {{rule, e.g. "Ask before adding any library or dependency: write it in APPROVALS.md."}}
- {{rule that protects real data, e.g. "Tests and manual runs never touch production data, live accounts or the user's real files; use a temp copy or a test place."}}
- An item is done only when its check in section 4 passes.
- Never delete or rewrite earlier `DEVLOG.md` entries.
- If this file conflicts with `CLAUDE.md` or with the user's message in this session, they win; record the conflict in `QUESTIONS.md`.

## 2. Context map

Read only what today's items need.

| What | Where |
|---|---|
| Spec / design doc | {{path}} |
| Source code | {{paths}} |
| Tests / checks | {{paths or commands}} |
| Conventions | {{CLAUDE.md, style guides, skills}} |
| Tools this work uses | {{MCP servers, CLIs, apps, e.g. "Studio MCP: Roblox_Studio"; or "none beyond git and the shell"}} |
| State files | {{folder}}: `BACKLOG.md`, `DEVLOG.md`, `HANDOFF.md`, `QUESTIONS.md`, `APPROVALS.md` |

## 3. Start of session

Do these in order before writing any code.

1. Read `HANDOFF.md`, then the last 3 entries of `DEVLOG.md` (newest at the bottom).
2. If the last `DEVLOG.md` entry still says `Status: in progress`, the previous session was interrupted: treat its plan and any uncommitted changes as leftover work in step 4, and mark that entry `Status: interrupted`.
3. Read `QUESTIONS.md` and `APPROVALS.md`. Apply every answer and decision the user added since the last run, then mark each one `applied (YYYY-MM-DD)`. An unanswered question keeps its "Assumed until answered" line in force.
4. Run `git status` and `git log --oneline -10`. If there are uncommitted changes from an earlier session: finish and commit them if they clearly belong to a backlog item and pass its check; otherwise commit them to a branch `wip/<date>-recovered`, switch back, and note it in `DEVLOG.md`. Never discard them.
5. Append today's `DEVLOG.md` entry now with the date and `Status: in progress`, so an interrupted session can be detected next time.
6. Environment check: {{commands or tool calls that prove the tools work and the baseline is green, e.g. "npm ci && npm test", "Studio MCP: confirm the place is open, run a playtest, zero console errors"}}. If the baseline is red, fixing it is today's first item. If a required tool is unreachable, work on items that don't need it and log the problem.
7. Choose today's plan: the top {{N, e.g. 3}} `todo` items in `BACKLOG.md` whose dependencies are `done` or earlier in today's plan. Write the plan into today's `DEVLOG.md` entry. If the plan is finished early, you may take the next item, up to the limit in section 7.

## 4. Work loop

For each item in today's plan:

1. Set it to `doing` in `BACKLOG.md`.
2. Write the smallest plan that finishes the item. If it can't fit in this session, split it in `BACKLOG.md` and take the first part.
3. Implement it following the conventions in section 2.
4. Verify: {{the project's check, e.g. "npm test && npm run lint", "playtest through MCP and read the console: zero errors, the feature behaves as the item's 'Done when' says"}}. If the check fails, fix and re-run. After 3 failed attempts, set the item to `blocked`, write the reason in `QUESTIONS.md`, and move on.
5. Set the item to `done` in `BACKLOG.md` and add any new work you discovered, with a priority.
6. Commit the item together with that `BACKLOG.md` change: {{commit rule, e.g. "one commit per item, message 'feat(<area>): <item title> [B-012]'"}}. The end-of-session state commit fills in the item's hash. Never amend commits.

## 5. When blocked

- A decision only the user can make: add it to `QUESTIONS.md` with the options, your recommendation, and what sessions will assume until it is answered. If that assumption is safe and reversible, go ahead with it; if not, block the item.
- An action that needs permission ({{list, e.g. "new dependency, deleting files, spending money, publishing, anything irreversible"}}): add it to `APPROVALS.md` and do not do it.
- {{"Assume nobody is watching: never wait for an answer. If the user is actively replying in this session, you may ask them directly, and still record the answer in QUESTIONS.md." — or, for attended-only projects: "Ask the user directly, and record the answer in QUESTIONS.md."}}
- Then continue with the next item. One blocked item never ends the session.

## 6. End of session

Always do all of this, even if little got done.

1. Every change that passed its check is committed.
2. Complete today's `DEVLOG.md` entry: set `Status: done`, list done items (with commit hashes), not done and why, discoveries. When the log passes about 15 entries, collapse the oldest into one line each under `## Earlier`.
3. Rewrite `HANDOFF.md`: status line, what works now, what is in progress, the next 3 actions, warnings and gotchas, number of open questions and approvals.
4. Re-order `BACKLOG.md` if priorities changed.
5. Commit the state files: `chore: daily session YYYY-MM-DD`. `git status` must be clean. {{push rule, e.g. "Then push to origin/main." or "Never push; the repo has no remote."}}
6. Report to the user in 10 lines or fewer: what was done, what is blocked, which questions or approvals need their answer, and milestone progress.

## 7. Limits

- Stop starting new items after {{limit, e.g. "4 items or about 3 hours of work"}}; finish the current one, then do section 6.
- Work only on `BACKLOG.md` items. New ideas go into `BACKLOG.md` as `idea`, not into code.
- {{project-specific limit, e.g. "Touch only src/, tools/ and the state files."}}

## Addenda

Dated changes the user asked for, newest first. Each one overrides the sections above where they conflict.
