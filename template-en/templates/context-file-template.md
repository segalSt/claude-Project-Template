# <Project name> — context for continuing development

Reusable template. This file is not a technical SPEC (that's in
`docs/SPEC.md`), but a prompt for quickly getting into the project in a new
session: what it is, where it lives, what's already done, what's next, which
pitfalls not to step on again.

## What it is

_One paragraph: what the project is, for whom, the key idea._

## Commands

Command definitions — `commands.md` (reusable, copied as
is). Project binding:
- Plan — `docs/plan.md`. **Layout** (see `plan-template.md`): either
  **monolith** (all steps in `plan.md`, each with its own `### Tasks`/
  `### Manual QA`), or **split** (`plan.md` = `## Step index`; step body in
  `plan/step-<N>.md`, committed ones in `plan/archive/step-<N>.md`). State
  here which layout is currently in use.
- Error/discrepancy log — `docs/errors.md`.
- Current step (for `-QA` without a parameter) — the "Right now" section below.

## Where it lives

```
<path to project>
```

## Right now (update at the end of each session)

The latest delta — `docs/devHistory.md`, Session N (don't retell the
content here, only a link + the critical minimum for continuing:
what is NOT committed, what to do next).

## Where to start in a new session

1. `guardrails.md` — hard rules, always first, cheap file.
2. `docs/devHistory.md` (at least the most recent entry).
3. `docs/SPEC.md` in full.
4. `docs/plan.md` — step-by-step plan; `docs/errors.md` — known
   discrepancies.
5. Run the tests/build, make sure the starting point works.
6. **After reading — produce a short brief** with EXPLICIT sections:
   **Project goal** (one line), **Done**, **Current state** (incl.
   uncommitted / environment gotchas), **Plan** (next steps), **Under
   discussion** (open TBD/Open), **Reminders**. Keep it short, 1-2 items per
   section — it's a sync point before work, not a retelling of the context.

## Stack and solution structure

_Languages, frameworks, folder structure, key architectural decisions
(briefly — details in SPEC.md)._

## How to work with me on this project

General conventions — `working-conventions.md` (reusable, copied
as is). Specifics of this particular project:
- Language of code/comments/commits: ___. Language of communication with the user: ___.
- _(other project specifics, if any)_

## Known environment gotchas

_Environment problems specific to this project, if any (tool
versions, known IDE bugs, etc.)._

## Files to start reading from

1. `guardrails.md`
2. `docs/devHistory.md`
3. `docs/SPEC.md`
4. `docs/plan.md`
5. `docs/errors.md`
6. _(key code files of this project)_
