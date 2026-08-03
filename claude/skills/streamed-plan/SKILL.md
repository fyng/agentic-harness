---
name: streamed-plan
description: Turn requirements into a concrete implementation plan, decomposed into parallel workstreams with every unit task labeled [easy]/[medium]/[hard] so downstream effort is calibrated. Use before delegating implementation work — when the user asks to plan a feature, fix, or refactor, or when a task needs sequencing and workstream breakdown before any code is written.
---

# Streamed Plan

Design the implementation plan in this session — you already have the repo context. Explore
only what you still need.

## You are in planning mode

**Research and design only. Do not implement anything.** This holds until the user approves
the plan.

Prohibited while planning:
- `Write`, `Edit`, `NotebookEdit` — no new files, no edits, not even a scratch file or a "quick fix" you spotted along the way
- Any state-changing shell command: `mkdir`, `touch`, `rm`, `cp`, `mv`, `git add`, `git commit`, `git checkout`, `pip install`, `npm install`
- Redirects (`>`, `>>`) and heredocs
- Delegating to `implementation` or any other writing subagent

Allowed: `Read`, `Grep`, `Glob`, `WebFetch`/`WebSearch`, `Explore`, `AskUserQuestion`,
`TodoWrite`, and read-only shell (`ls`, `find`, `cat`, `git status`, `git log`, `git diff`).

Do not touch the codebase, even if the change looks trivial or the user's request sounds
urgent. Noticing a bug mid-plan is a plan item, not a detour. The deliverable of this skill is
a plan the user reads and approves — nothing else.

## Skip condition — trivial work

Size the task before planning it. Skip decomposition when any of these hold:

- The work is one atomic unit (read → change → validate) — nothing to parallelize.
- It's already done, or the diff is a handful of lines in one or two files.
- No design fork exists: there is one obvious way to do it.

Explicitly state that delegation not needed, name the unit task with its difficulty label, and hand off immediately. When the task is only *partly* trivial, do the trivial task first, then plan the non-trivial remainder.

## Planning Process

1. **Understand the requirements.** Resolve decision forks with `AskUserQuestion` before planning around a guess.
2. **Explore what's missing.** Find existing patterns and conventions; identify a similar
   feature as reference; trace the relevant code paths. Read files directly. Fan out with
   `Explore` only when the search is broad and you need the conclusion, not the file dumps.
3. **Design the solution.** Weigh trade-offs; state decisions directly, no hedging. Follow
   existing patterns unless there's a reason not to.
4. **Decompose into workstreams.**
   - Break work into atomic units: read → change → validate.
   - Group into parallel, non-blocking workstreams; make dependencies and sequencing explicit.
   - Every delegated unit task ends on a **synchronous** check — tests, a lint pass, a short
     foreground command.
   - Long-running work (SLURM `sbatch`, servers, training runs, anything backgrounded) belongs
     to the **main agent**, never inside a delegated unit task. Record the exact command, the
     pass and fail signatures, and where the result lands.
5. **Label difficulty.** Every unit task carries `[easy]`, `[medium]`, or `[hard]`:
   - **Easy** — variable change, targeted edit. Just do it. No over-engineering, no verification unless critical.
   - **Medium** — new function, adapting existing code. Implement, then verify.
   - **Hard** — new feature, requires taste. Build understanding, think through design decisions, verify after.
   - Route publication figures and manuscript plots to the `nature-publication-figures` agent, not `implementation`.
6. **Record the plan.** Track the steps with `TodoWrite`, one entry per unit task with its label.

## Handoff

Present the plan and stop. Implementation begins only once the user approves —
delegated to the `implementation` agent, one workstream at a time. 

Keep handoff terse — bullets, no filler sections. End with list of critical files for implementation.
