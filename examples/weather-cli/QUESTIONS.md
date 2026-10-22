# Questions for the user

Answer under **Answer:** in your own words. The next session applies the answer and marks it `applied`. Until then, sessions follow the "Assumed until answered" line.

## Q-002 · 2026-10-21 · open
**Question:** The sparkline uses Unicode blocks (▁▂▃▅▇), which some Windows terminals show as boxes. What should happen there?
**Options:** A: detect and fall back to ASCII (`_.-'`) · B: Unicode only, document it · C: drop the sparkline
**Recommendation:** A, it costs one small function and keeps the feature everywhere.
**Assumed until answered:** item blocked.
**Answer:**

## Q-001 · 2026-10-19 · applied (2026-10-21)
**Question:** Which forecast API?
**Options:** A: Open-Meteo (free, no key) · B: OpenWeather (key needed)
**Recommendation:** A, no key to manage for users.
**Assumed until answered:** A, code kept behind one `fetch_forecast()` function.
**Answer:** A, Open-Meteo.
