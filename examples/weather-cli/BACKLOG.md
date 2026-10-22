# Backlog: weather-cli

Status: `todo` · `doing` · `blocked` · `done` · `idea`
Priority: `P1` needed for the current milestone · `P2` after it · `P3` someday
Order matters: the first `todo` item whose dependencies are `done` is next.

| ID | P | Status | Item | Done when | Depends on | Commit |
|---|---|---|---|---|---|---|
| B-001 | P1 | done | Project skeleton: `pyproject.toml`, `src/wx/`, `wx` entry point | `pip install -e .` works, `wx --version` prints 0.1.0 | | a41c9e2 |
| B-002 | P1 | done | Geocode a city name to lat/lon | `wx lisbon --debug` prints 38.72, -9.14; test with fixture | B-001 | 7d03b1f |
| B-003 | P1 | done | Fetch today + 3 days forecast | test with recorded fixture passes, no network in tests | B-002 | c96e2a0 |
| B-004 | P1 | done | Pretty output with colours | snapshot test of the output passes | B-003 | 18fa7d5 |
| B-005 | P1 | done | 16-colour fallback (addendum 2026-10-21) | `TERM=xterm wx lisbon` uses no 24-bit codes | B-004 | e02b6c4 |
| B-006 | P1 | blocked | Temperature sparkline | snapshot test passes | B-004 | |
| B-007 | P1 | todo | `--units metric|imperial` flag | both modes tested | B-003 | |
| B-008 | P1 | todo | Friendly error for unknown city | `wx qwerty` exits 1 with one clear line | B-002 | |
| B-009 | P1 | todo | `wx --help` documents every flag | help text reviewed against `docs/spec.md` | B-007 | |
| B-010 | P2 | idea | Cache the last result for 10 minutes | | | |
