# Daily Master Prompt

A Claude Code plugin that turns any project into a self-running daily work session.

You explain the project once (or point Claude at what's already there). The plugin writes a single `DAILY_PROMPT.md` plus five small state files into your project. From then on, every day, a fresh Claude session only needs one line:

```
Read games/speed-escape/DAILY_PROMPT.md and execute it.
```

It reads where the last session stopped, does the next items in the backlog, checks its own work, commits, and leaves a handoff for tomorrow.

## Why it works with one line

A normal chat forgets everything when it ends. This plugin moves the project's memory out of the chat and into files that live in the repo:

| File | What it holds | Who updates it |
|---|---|---|
| `DAILY_PROMPT.md` | Mission, current milestone and its "done" definition, hard rules, where things are, and the exact session procedure | You (via `create`) when goals change |
| `BACKLOG.md` | Ordered work items with IDs, status, and how each one is checked | Every session |
| `DEVLOG.md` | Append-only log: what each session planned, did, and learned | Every session |
| `HANDOFF.md` | Rewritten each session: what works, what's in progress, next 3 actions, gotchas | Every session |
| `QUESTIONS.md` | Decisions only you can make, each with options and a recommendation | Sessions ask, you answer |
| `APPROVALS.md` | Risky actions (new dependency, deletion, publishing…) waiting for your yes/no | Sessions ask, you decide |

`DAILY_PROMPT.md` is self-contained: its first instruction is to read `HANDOFF.md` and the latest `DEVLOG.md` entries, then apply your answers from `QUESTIONS.md` and `APPROVALS.md`. So "read it and execute it" is all a new session needs, with no copy-pasting and no re-explaining. The line even works without the plugin installed, in any tool that can read files.

Sessions never stall on a blocker: they write the question down, move to the next item, and you answer whenever you want. The next session picks your answer up.

## Install

In Claude Code:

```
/plugin marketplace add CAFEERON21br18/daily-master-prompt
/plugin install daily-master-prompt@cafeeron-plugins
```

Or from your shell:

```bash
claude plugin marketplace add CAFEERON21br18/daily-master-prompt
claude plugin install daily-master-prompt@cafeeron-plugins
```

Update later with `claude plugin update daily-master-prompt@cafeeron-plugins`.

## Use

### 1. Create the prompt (once per project)

```
/daily-master-prompt:create games/speed-escape  A Halloween obby on Roblox. Goal: launch-ready build this week.
```

You can give a folder, an explanation, both, or neither. Claude then:

1. Reads what already exists: README, CLAUDE.md, specs and design docs, git history, file tree, build and test scripts, connected tools.
2. Asks at most a few questions about what it couldn't find (milestone and "done" definition, how work is verified, hard rules, attended or unattended).
3. Writes `DAILY_PROMPT.md` with your real paths, commands and rules, and creates the state files (merging into any that already exist).
4. Runs the first session right away. That first run is the prompt's test: anything ambiguous gets fixed in the prompt.
5. Gives you the daily line.

You can also just ask in plain words: "set up a daily master prompt for this project".

### 2. Every day after that

Type the line:

```
Read games/speed-escape/DAILY_PROMPT.md and execute it.
```

or the shortcut:

```
/daily-master-prompt:run games/speed-escape
```

Then read the short report at the end, answer anything in `QUESTIONS.md` / `APPROVALS.md`, and you're done.

### 3. Run it without being there (optional)

Because the whole session is one line, it can be the prompt of a scheduled task (for example a Claude Code Desktop scheduled task or a cloud routine). Set the prompt to `Read <folder>/DAILY_PROMPT.md and execute it.` The prompt is written for unattended runs: blockers go to `QUESTIONS.md` and `APPROVALS.md` instead of waiting for you.

### 4. Change direction

```
/daily-master-prompt:create games/speed-escape  add an addendum: from today, prioritise mobile UI over new levels
```

When `DAILY_PROMPT.md` exists, `create` edits it (or adds a dated entry under `## Addenda`) and keeps all state.

## What's in this repository

```
.claude-plugin/
  plugin.json          plugin manifest
  marketplace.json     makes this repo installable as a marketplace
skills/
  create/
    SKILL.md           builds or updates a project's daily master prompt
    templates/         DAILY_PROMPT, BACKLOG, DEVLOG, HANDOFF, QUESTIONS, APPROVALS
  run/
    SKILL.md           /daily-master-prompt:run <folder>
```

## Develop locally

```bash
claude plugin validate --strict .
claude --plugin-dir .
```

Bump `version` in `.claude-plugin/plugin.json` for every release, otherwise `claude plugin update` keeps users on the old copy.

## Privacy and security

- This plugin collects no data and sends nothing anywhere. It has no MCP servers, hooks, scripts or network access, and the author runs no service that could receive data.
- Its skills only read files in your project and write Markdown files (`DAILY_PROMPT.md`, `BACKLOG.md`, `DEVLOG.md`, `HANDOFF.md`, `QUESTIONS.md`, `APPROVALS.md`) inside your project folder.
- The plugin ships no executable code. During a session, Claude may run your project's own commands (tests, builds, git commits) through its normal tools, under your Claude Code permission settings. Publishing, deploying, pushing and adding dependencies are written to `APPROVALS.md` for you to decide, unless your own project rules allow them.
- Questions or concerns: open an issue on this repository.

## License

MIT
