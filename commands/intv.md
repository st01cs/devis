---
allowed-tools: AskUserQuestion, Read, Glob, Grep, Write, Edit
argument-hint: [plan-file]
description: Interview to flesh out a plan/spec
---

Here's the current plan:

@$ARGUMENTS

Interview me in detail using the AskUserQuestion tool about literally anything: technical implementation, UI & UX, concerns, tradeoffs, etc. but make sure the questions are not obvious.

**_ Interview me in English, and generate plan in English. _**

Be very in-depth and continue interviewing me continually until it's complete, then create three files in the same directory as `$ARGUMENTS`:

| File           | Purpose                                  | Reference                                    |
| -------------- | ---------------------------------------- | -------------------------------------------- |
| `task_plan.md` | Phases, progress, decisions              | ${CLAUDE_PLUGIN_ROOT}/templates/task_plan.md |
| `findings.md`  | Research, discoveries, interview content | ${CLAUDE_PLUGIN_ROOT}/templates/findings.md  |
| `progress.md`  | Session log, test results                | ${CLAUDE_PLUGIN_ROOT}/templates/progress.md  |

**_ Important: Where Files Go _**

- **Templates** are stored in the skill directory at `${CLAUDE_PLUGIN_ROOT}/templates/`
- **Your planning files** (`task_plan.md`, `findings.md`, `progress.md`) should be created in the same directory as `$ARGUMENTS`

| Location                                       | What Goes There                              |
| ---------------------------------------------- | -------------------------------------------- |
| Skill directory (`${CLAUDE_PLUGIN_ROOT}/`)     | Templates, scripts, reference docs           |
| Your plan directory (the same as `$ARGUMENTS`) | `task_plan.md`, `findings.md`, `progress.md` |
