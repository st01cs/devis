---
allowed-tools: Read, Glob, Grep, Write, Edit, Bash, WebFetch, WebSearch
argument-hint: [plan-file]
description: Implement the plan to code
hooks:
  PreToolUse:
    - matcher: "Write|Edit|Bash"
      hooks:
        - type: command
          command: "cat ${PLAN_ROOT}/task_plan.md 2>/dev/null | head -30 || true"
  PostToolUse:
    - matcher: "Write|Edit"
      hooks:
        - type: command
          command: "echo '[impl] File updated. If this completes a phase, update task_plan.md status.'"
  Stop:
    - hooks:
        - type: command
          command: "${CLAUDE_PLUGIN_ROOT}/scripts/check-complete.sh"
---

Here's the current plan:

@$ARGUMENTS

Export the directory containing the $ARGUMENTS file as an environment variable named PLAN_ROOT.

# Critical rules

- Write code strictly according to the plan, do not exceed the scope, and proceed in small steps quickly.
- Work like Manus: Use persistent markdown files as your "working memory on disk."

## The Core Pattern

```
Context Window = RAM (volatile, limited)
Filesystem = Disk (persistent, unlimited)

→ Anything important gets written to disk.
```

## File Purposes

PLAN_ROOT is the directory where the @$ARGUMENTS file is located.

| File                        | Purpose                     | When to Update      |
| --------------------------- | --------------------------- | ------------------- |
| `${PLAN_ROOT}/task_plan.md` | Phases, progress, decisions | After each phase    |
| `${PLAN_ROOT}/findings.md`  | Research, discoveries       | After ANY discovery |
| `${PLAN_ROOT}/progress.md`  | Session log, test results   | Throughout session  |

## Quick Start

Before ANY complex task:

1. **Read `task_plan.md`, `findings.md`, `progress.md`** in `${PLAN_ROOT}`
2. **Re-read plan before decisions** — Refreshes goals in attention window
3. **Update after each phase** — Mark complete, log errors

> **Note:** All three planning files should be saved in your current working directory (`${PLAN_ROOT}`), not in the skill's installation folder.

## Detail Steps

### 1. Read Plan First

Never start a complex task without `task_plan.md`. Non-negotiable.

### 2. The 2-Action Rule

> "After every 2 view/browser/search operations, IMMEDIATELY save key findings to text files."

This prevents visual/multimodal information from being lost.

### 3. Read Before Decide

Before major decisions, read the plan file. This keeps goals in your attention window.

### 4. Update After Act

After completing any phase:

- Mark phase status: `in_progress` → `complete`
- Log any errors encountered
- Note files created/modified

### 5. Log ALL Errors

Every error goes in the plan file. This builds knowledge and prevents repetition.

```markdown
## Errors Encountered

| Error             | Attempt | Resolution             |
| ----------------- | ------- | ---------------------- |
| FileNotFoundError | 1       | Created default config |
| API timeout       | 2       | Added retry logic      |
```

### 6. Never Repeat Failures

```
if action_failed:
    next_action != same_action
```

Track what you tried. Mutate the approach.

## The 3-Strike Error Protocol

```
ATTEMPT 1: Diagnose & Fix
  → Read error carefully
  → Identify root cause
  → Apply targeted fix

ATTEMPT 2: Alternative Approach
  → Same error? Try different method
  → Different tool? Different library?
  → NEVER repeat exact same failing action

ATTEMPT 3: Broader Rethink
  → Question assumptions
  → Search for solutions
  → Consider updating the plan

AFTER 3 FAILURES: Escalate to User
  → Explain what you tried
  → Share the specific error
  → Ask for guidance
```

## Read vs Write Decision Matrix

| Situation             | Action                  | Reason                        |
| --------------------- | ----------------------- | ----------------------------- |
| Just wrote a file     | DON'T read              | Content still in context      |
| Viewed image/PDF      | Write findings NOW      | Multimodal → text before lost |
| Browser returned data | Write to file           | Screenshots don't persist     |
| Starting new phase    | Read plan/findings      | Re-orient if context stale    |
| Error occurred        | Read relevant file      | Need current state to fix     |
| Resuming after gap    | Read all planning files | Recover state                 |

## The 5-Question Reboot Test

If you can answer these, your context management is solid:

| Question             | Answer Source                 |
| -------------------- | ----------------------------- |
| Where am I?          | Current phase in task_plan.md |
| Where am I going?    | Remaining phases              |
| What's the goal?     | Goal statement in plan        |
| What have I learned? | findings.md                   |
| What have I done?    | progress.md                   |

## Advanced Topics

- **Manus Principles:** See ${CLAUDE_PLUGIN_ROOT}/refs/manus.md
- **Real Examples:** See ${CLAUDE_PLUGIN_ROOT}/refs/examples.md

## Anti-Patterns

| Don't                           | Do Instead                      |
| ------------------------------- | ------------------------------- |
| Use TodoWrite for persistence   | Create task_plan.md file        |
| State goals once and forget     | Re-read plan before decisions   |
| Hide errors and retry silently  | Log errors to plan file         |
| Stuff everything in context     | Store large content in files    |
| Start executing immediately     | Create plan file FIRST          |
| Repeat failed actions           | Track attempts, mutate approach |
| Create files in skill directory | Create files in your project    |

## Scripts

Helper scripts for automation:

- `scripts/check-complete.sh` — Verify all phases complete
