---
name: run
description: Run today's work session for a project from its DAILY_PROMPT.md.
argument-hint: "[project folder]"
disable-model-invocation: true
---

# Run the daily master prompt

1. Find the prompt: `$ARGUMENTS/DAILY_PROMPT.md`. With no argument, search the repository for `DAILY_PROMPT.md`; if there are several, ask which project.
2. If none exists, say so and offer `/daily-master-prompt:create`.
3. Read the whole file, then execute it as your instructions for this session, from its first step through its end-of-session steps.
4. `CLAUDE.md` and the user's messages in this session take precedence over the prompt. When they conflict, follow them and record the conflict in the project's `QUESTIONS.md`.
