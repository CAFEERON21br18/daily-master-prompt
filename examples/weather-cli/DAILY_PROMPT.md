# Daily Master Prompt: weather-cli

A fresh session runs this file with:

```
Read examples/weather-cli/DAILY_PROMPT.md and execute it.
```

Everything this session needs is in this file and the state files it names. Do not rely on memory of earlier conversations. Paths below are relative to the repository root. Get today's date from the system (`date +%Y-%m-%d`) and use it for every dated entry.

> This is an example of what `/daily-master-prompt:create` writes, shown after three daily sessions. The project is fictional.

## 0. Mission

`weather-cli` is a small Python command-line tool: `wx lisbon` prints today's forecast and the next 3 days in the terminal, with colours and an ASCII sparkline for temperature. It is for people who live in the terminal and want the weather in one second.

- **Current milestone:** v0.1, first release on PyPI
- **Done means:** all P1 items done, `pytest` and `ruff check` green, `wx --help` documents every flag, a user run-through checklist is written in `HANDOFF.md`
- **When done is reached:** put `MILESTONE READY` as the first line of `HANDOFF.md`, say it in the end-of-session report, and only take P2 items afterwards.

## 1. Ground rules

- Never publish to PyPI; the user does that.
- Ask before adding any dependency: write it in `APPROVALS.md`.
- Tests never call the real weather API; they use the recorded responses in `tests/fixtures/`.
- An item is done only when its check in section 4 passes.
- Never delete or rewrite earlier `DEVLOG.md` entries.
- If this file conflicts with `CLAUDE.md` or with the user's message in this session, they win; record the conflict in `QUESTIONS.md`.

## 2. Context map

Read only what today's items need.

| What | Where |
|---|---|
| Spec | `docs/spec.md` |
| Source code | `src/wx/` |
| Tests / checks | `tests/`, `pytest -q`, `ruff check .` |
| Conventions | `CLAUDE.md` |
| Tools this work uses | none beyond git, Python 3.12 and the shell |
| State files | `examples/weather-cli/`: `BACKLOG.md`, `DEVLOG.md`, `HANDOFF.md`, `QUESTIONS.md`, `APPROVALS.md` |

## 3. Start of session

1. Read `HANDOFF.md`, then the last 3 entries of `DEVLOG.md`.
2. If the last `DEVLOG.md` entry still says `Status: in progress`, the previous session was interrupted: treat its plan and any uncommitted changes as leftover work, and mark that entry `Status: interrupted`.
3. Read `QUESTIONS.md` and `APPROVALS.md`. Apply every new answer and decision, then mark each one `applied (YYYY-MM-DD)`.
4. Run `git status` and `git log --oneline -10`. Never discard uncommitted work: finish it or move it to `wip/<date>-recovered`.
5. Append today's `DEVLOG.md` entry with `Status: in progress`.
6. Environment check: `pip install -e .[dev] && pytest -q && ruff check .`. If red, fixing it is today's first item.
7. Choose today's plan: the top 3 `todo` items in `BACKLOG.md` whose dependencies are `done`. Write the plan into today's entry.

## 4. Work loop

For each item: set it to `doing`, write the smallest plan, implement, then verify with `pytest -q && ruff check .` plus the item's own "Done when". After 3 failed attempts, set it to `blocked`, explain in `QUESTIONS.md`, move on. Then set it to `done` and commit: `feat(<area>): <title> [B-NNN]`. Never amend.

## 5. When blocked

- A decision only the user can make goes to `QUESTIONS.md` with options, a recommendation, and what sessions assume meanwhile.
- New dependency, deleting files, publishing: `APPROVALS.md`, and do not do it.
- Assume nobody is watching: never wait for an answer. Continue with the next item.

## 6. End of session

1. Every change that passed its check is committed.
2. Complete today's `DEVLOG.md` entry with `Status: done`.
3. Rewrite `HANDOFF.md`.
4. Re-order `BACKLOG.md` if priorities changed.
5. Commit the state files: `chore: daily session YYYY-MM-DD`. Never push; the user pushes.
6. Report in 10 lines or fewer.

## 7. Limits

- Stop starting new items after 4 items or about 3 hours of work.
- Work only on `BACKLOG.md` items. New ideas go in as `idea`.

## Addenda

### 2026-10-21
Colours must also work in terminals without true colour: fall back to 16 colours.
