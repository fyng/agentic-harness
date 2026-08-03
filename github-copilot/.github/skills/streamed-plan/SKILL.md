---
name: streamed-plan
description: Turn requirements into a concrete implementation plan, decomposed into parallel workstreams with every unit task labeled [easy]/[medium]/[hard] so downstream effort is calibrated. Use before delegating implementation work when a task needs sequencing and workstream breakdown before code is written.
---

# Streamed Plan

Design the implementation plan in this session. Explore only what you still need.

## You are in planning mode

**Research and design only. Do not implement anything.** This holds until the user approves the plan.

Prohibited while planning:
- File-editing tools or any other way of changing files, even for a quick fix.
- State-changing shell commands such as `mkdir`, `touch`, `rm`, `cp`, `mv`, `git add`, `git commit`, `git checkout`, or package installation.
- Redirects (`>`, `>>`) and heredocs.
- Delegating to an implementation or other writing subagent.

Allowed: reading, searching, browsing, asking the user questions, task-list updates, and read-only shell commands.

Do not touch the codebase, even if the change looks trivial or urgent. Noticing a bug mid-plan is a plan item, not a detour. The deliverable of this skill is a plan the user reads and approves, nothing else.

## Skip condition: trivial work

Skip decomposition when:
- The work is one atomic unit: read, change, validate.
- It is already done, or the diff is a handful of lines in one or two files.
- No design fork exists: there is one obvious way to proceed.

Explicitly state that delegation is not needed, name the unit task with its difficulty label, and hand off immediately. When the task is only partly trivial, do the trivial task first, then plan the non-trivial remainder.

## Planning Process

1. **Understand the requirements.** Resolve decision forks with the user before planning around a guess.
2. **Explore what is missing.** Find existing patterns and conventions; identify a similar feature as reference; trace the relevant code paths. Read files directly. Use exploration agents only when the search is broad and you need the conclusion, not file dumps.
3. **Design the solution.** Weigh trade-offs; state decisions directly, without hedging. Follow existing patterns unless there is a reason not to.
4. **Decompose into workstreams.**
   - Break work into atomic units: read -> change -> validate.
   - Group into parallel, non-blocking workstreams; make dependencies and sequencing explicit.
   - Every delegated unit task ends on a synchronous check: tests, a lint pass, or a short foreground command.
   - Long-running work belongs to the main agent, never inside a delegated unit task. Record the exact command, the pass and fail signatures, and where the result lands.
5. **Label difficulty.** Every unit task carries `[easy]`, `[medium]`, or `[hard]`:
   - **Easy**: variable change or targeted edit. Just do it. Do not over-engineer.
   - **Medium**: new function or adapting existing code. Implement, then verify.
   - **Hard**: new feature requiring design decisions. Build understanding and verify after completion.
   - Route publication figures and manuscript plots to the `nature-publication-figures` agent, not `implementation`.
6. **Record the plan.** Track the steps with the available task-list tool, one entry per unit task with its label.

## Handoff

Present the plan and stop. Implementation begins only once the user approves, delegated to the `implementation` agent one workstream at a time.

Keep handoff terse: bullets, no filler sections. End with the critical files for implementation.
