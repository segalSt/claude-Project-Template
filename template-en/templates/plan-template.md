# Implementation plan

Reusable template. A step-by-step plan of the remaining work — each step
= one logical commit where possible.

**Model (see `README.md`):** this is the plan of the CURRENT project/version —
its phase-steps (spec → plan → implementation → verification → completion),
each step = 1+ tasks. It is NOT a registry of different tasks and NOT a
history: finished work and past versions go to `devHistory.md`, not here.
`-plan` shows the current version's steps.

**Actuality rule:** this file must always reflect the current
state of the project. Any project change that affects the status of a step or
the design description inside a step is reflected right here, in the same pass.
Discrepancies found when reconciling the plan against the SPEC/code — are recorded in
`errors.md` (not fixed silently), status `Open`/`Fixed`. Any subsequent
edit must also verify that already-`Fixed` records have not returned.

**Step reference rule:** when discussing in chat any action
that relates not to the current step — state the number of the step it
relates to. If an action is not covered by any existing step —
say so explicitly.

**Test catalog rule** (if the project has automated tests):
`tests-catalog.csv` (number, file, test, what it checks) is updated in the
same pass as any test change.

**When to split the plan into files (scalable layout).** While the plan is
small, all steps live directly in this file. Once `plan.md` stops being
cheap to read — move each step's body into its own file: active ones to
`plan/step-<N>.md`, committed ones to `plan/archive/step-<N>.md`. `plan.md`
then keeps only: meta-rules + `## Step index` (number, name, STEP-LEVEL
status, link to file) + cross-cutting sections (TBD/Errors/Out-of-scope/Git).
A step's body = `### Tasks` (the task table — the single source of task
statuses, rule 11) + design + `### Manual QA`.

**Explicit threshold (guideline, not dogma) — split when ANY of these holds:**
- `plan.md` exceeds **~400 lines**; or
- any SINGLE step's body exceeds **~150 lines**; or
- there are more than **~10** steps with substantial bodies.

Trigger sign: every read of the plan drags in hundreds of lines of already
closed/stale steps. The split is a mechanical operation (better done by a
script/shell than by hand-retyping); closed steps live safely in git
history, and the detailed multi-hundred-line history of a closed step need
not be copied verbatim — leave the `### Tasks` table in its file plus a
pointer to git.

---

## Step 0 — <name of the first step>

_Description: what needs to be done, which files are affected, what it relies on._

**Manual QA (UI, not covered by automated tests):**
- _Items specific to this step — fill in with specifics._

**Commit:** `<draft commit message>`

---

<!-- Add steps 1, 2, ... as the plan is discussed with the user. -->

## TBD (discuss before applying)

Topics marked with the `-tbd <text>` command (`commands.md`) — recorded
immediately so they're not lost; implementation/plan changes for them do not start
without explicit discussion and a decision. Status: `Open` (awaiting discussion) →
`Discussing` (being discussed) → `Accepted` (accepted, moved as a separate
step/edit — note the step number) or `Rejected` (rejected — stays in the
table for history, not deleted).

| № | Text | Status | Where it went / why rejected |
|---|---|---|---|

## Errors

See `errors.md` (or the section below, if the project decided not to create
a separate file — then the error table is kept right here, in the same
format).

## Not part of this plan

_Explicitly list what has been consciously deferred/rejected, so as not to
revisit the same question in circles._
