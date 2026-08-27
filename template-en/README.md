# Purpose and working model (context template)

A reusable set of context files for working with an AI assistant on a software
project. Keeps rules, plan and history organized so the assistant stays
disciplined over long sessions and doesn't drift as the context grows. This is
the **base understanding** — every other template file follows it.

## Principle (how the work is organized)

The template describes work on **ONE project/version at a time**, broken into
base phases/steps:

1. **Spec (interview)** — define WHAT we're doing (`docs/SPEC.md`).
2. **Plan** — decompose the spec into implementation steps (`docs/plan.md`).
3. **Step-by-step implementation** — one step at a time.
4. **Verification** — tests + manual QA.
5. **Completion** — commit/release the version; move to the next version.

Each step/phase = **1+ tasks**.

### Unit of work (scope)

"One project/version" is a scope, and it can be one of two kinds:
- **a whole project** (e.g. tagsExpert — the whole app);
- **a single work item** — a user story / bug from a DevOps board inside a
  large project (e.g. Kolnatun / task 28476).

In both cases the context runs **ONE whole thing** — one coherent unit of work,
broken into phases. **No parallel independent tasks in one context** (you can't
run 28476 and 16446 at once): a different item = a different context; finished/
past items go to history.

Key consequences (must be honored by `plan.md`, the commands, `migration.md`,
`new-project-setup.md`):

- **`plan.md` is the plan of the CURRENT project/version** (its phase-steps),
  NOT a registry of different tasks and NOT a history.
- **`-plan` shows the current version's steps** (what to do per plan), not a
  list of past/unrelated tasks.
- **Completed work and past versions → history** (`docs/devHistory.md`), NOT
  the plan. Separate independent past tasks are history, not rows of the
  current plan.
- When a version is done — the plan is cleared / moves to the next version, and
  what's done goes to history.

## Files

- entry point `<Project>Prompt.md`, `commands.md`, `working-conventions.md`,
  `actualization-rules.md`;
- `docs/SPEC.md`, `docs/plan.md` (+ `docs/plan/` when it grows — see the
  threshold in `templates/plan-template.md`), `docs/errors.md`,
  `docs/qa-findings.md`, `docs/devHistory.md` (+ `docs/history/` when it
  grows — same principle, threshold in `templates/devHistory.md`).

## Entry points

- **New project from scratch** → `new-project-setup.md` (its Step -1 forks
  to `/wayfinder` first if the idea is too big/foggy for one session).
- **Migrate an existing project** to this structure → `migration.md`
  (command `-migrate`/`-миграция`).
- RU/EN template sync → `SYNC.md`.
