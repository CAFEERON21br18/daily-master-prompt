# Devlog: weather-cli

Append-only. One entry per session, newest at the bottom. Each entry starts with `Status: in progress` and ends the session as `Status: done`.

## 2026-10-19 · setup

Status: done

- Created the daily master prompt and state files.
- Context used: `docs/spec.md`, `CLAUDE.md`, empty repo with README.
- Assumptions (each one also in QUESTIONS.md): Q-001 forecast API.
- First run: B-001 done (a41c9e2). Fixed the prompt: environment check now installs dev extras.

## 2026-10-20

Status: done

- Plan: B-002, B-003, B-004.
- Done: B-002 (7d03b1f), B-003 (c96e2a0), B-004 (18fa7d5).
- Discovered: the API returns temperatures as strings for some cities; parsing handles both.
- Asked: A-001 (add `rich` for colours). Wrote colours with plain ANSI codes meanwhile, so nothing is blocked.

## 2026-10-21

Status: done

- Applied: Q-001 answer (Open-Meteo, no key). Addendum: 16-colour fallback added as B-005.
- Plan: B-005, B-006, B-007.
- Done: B-005 (e02b6c4).
- Blocked: B-006, the sparkline needs Unicode block characters that the Windows test terminal renders as boxes. Asked Q-002.
- Not started: B-007, session limit reached after B-006 attempts.
