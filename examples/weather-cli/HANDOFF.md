**Status:** 5 of 9 P1 items done · 1 blocked · on track for v0.1

# Handoff: weather-cli

Rewritten at the end of every session. Last updated: 2026-10-21

## What works now
- `wx lisbon` prints today and the next 3 days, coloured, with a 16-colour fallback.
- City names resolve through Open-Meteo geocoding; tests run offline from fixtures.

## In progress
- Nothing. B-006 (sparkline) is blocked on Q-002.

## Next 3 actions
1. B-007: `--units metric|imperial`.
2. B-008: friendly error for unknown cities.
3. B-009: `wx --help` for every flag.

## Warnings and gotchas
- Some API responses give temperatures as strings: always go through `parse_temp()`.
- Snapshot tests fail if `NO_COLOR` is set in your shell.

## Waiting on the user
- Open questions: 1 (Q-002, see QUESTIONS.md)
- Pending approvals: 1 (A-001, see APPROVALS.md)
