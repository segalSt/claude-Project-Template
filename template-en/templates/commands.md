# Assistant commands

Reusable file — not tied to a specific project, can be
copied into other repositories as-is. Short commands that the
user types directly into chat; the assistant must recognize them in any
session where this file is connected.

For commands that need project context (`-QA`), the project file itself
(for example, `tegExpertPrompt.md` or equivalent) must explicitly state
where the project plan/list of steps lives (usually `plan.md` or `docs/plan.md`).

**Plan layout (scalable, see `plan-template.md`).** The commands below work
with EITHER of two layouts:
- **Monolith** (small project): all steps live directly in `plan.md`, each
  with its own `### Tasks` and `### Manual QA` inside.
- **Split** (large project, past the threshold): `plan.md` = meta + `## Step
  index` (№, name, step-level status, link); each step's body is in its own
  file `plan/step-<N>.md` (committed ones — `plan/archive/step-<N>.md`), with
  `### Tasks` + design + `### Manual QA` inside.

Source rule: a command always reads from wherever the step currently lives —
the `### Tasks` / `### Manual QA` section in `plan.md` (monolith) or in the
step file (split). The project context file records which layout is in use.

**Terminology:** the plan consists of **steps** (the top-level planning
unit, usually one commit). Within a step — **tasks** (shown in
chat as a table with a row number, see the rule below) — concrete
work items within a step. `-start <number>` — the number of the task from
the last shown table, not the step number.

---

_Convention: a command is a single-dash token (`-plan`); its arguments/flags use **double dash** (`--s`, `--fix`). Positional args (numbers/text) take no dash._

- **`-commands`** — show in chat a list of all commands from this file
  (name + a one-line description), execute nothing.
  - **`--a`** — create a NEW command: ask the user for its text (name) and
    possible arguments, add the definition to this file (`commands.md`),
    then update the project context file (the command list) and both
    language copies of the template (RU + EN mirror, per the token map).
    Only after explicit text from the user.
  - **`--full`** — show a table of all commands and their arguments with full
    descriptions (command · argument · description); vs bare `-commands` (name + one line).

- **`-QA [number]`** — output to chat the "Manual QA" checklist for a step of the project
  plan. Source — the step's `### Manual QA` section (in `plan.md` for the
  monolith, or in the step file for the split layout).
  - With a parameter (for example `-QA 3`, `-QA 0.5`) — take the step with this number
    directly, without further determination.
  - Without a parameter — the CURRENT step, determined in this order: (1) explicitly
    named by the user in this or a recent message; (2) the step
    described as current/active in the project context file; (3)
    if neither applies — the first step top to bottom in the plan that has no
    "done"/"committed" mark in its status.
  - If the number doesn't exist in the plan, or without a parameter it's still
    ambiguous — ask the user which step is meant,
    rather than guessing.

- **`-plan`** — show the list of all project plan steps with the computed
  status of each (`new`/`in progress`/`done`/`committed` — from the mark in
  the step heading, and in its absence — from the general context; if
  unclear, mark it with a question in the output, don't make it up). Default
  format (without parameters) — a table: step №, name, status, a one-line
  summary, without breaking down into tasks. Source for a bare `-plan` in the
  split layout — `## Step index` in `plan.md`; in the monolith — the step
  headings in `plan.md`.

  Parameters (can be combined):
  - **`--s <number>`** — plan for this step only: step status + a table
    of its tasks (the step's `### Tasks` section — in `plan.md` or in the
    step file).
  - **`--a`** — the same, but for the ACTIVE (current) step — the same
    determination order as `-QA` without a parameter / as the context
    set by `-start --step <number>`.
  - **`--m <module>`** — an additional filter by module/screen (the
    "Module/screen" column in the task table) — keeps only tasks with this
    module. Without `--s`/`--a` alongside — filters all tasks across all steps
    at once.
  - **`--c`** — concise view: a list of steps WITHOUT breaking down into tasks, even
    if `--s`/`--a` are specified — `--c` always wins.
  - **`--hide-done`** — hide all finished tasks from the table (rows whose
    status starts with "Done"/the project's equivalent of "finished").
    Other statuses (not started, in progress, cancelled, etc.) remain
    visible. Combines with `--s`/`--a`/`--m`; makes no sense with `--c`.
  - If the step specified with `--s <number>` doesn't exist in the plan —
    ask, don't guess.

- **`-tbd <text>`** (to be discussed) — record a topic that needs to be
  discussed BEFORE anything about it is applied:
  1. Immediately add a row to the project plan (`plan.md`/equivalent), the
     "## TBD (discuss before applying)" section — the text, status `Open`.
     Implement nothing and change nothing in the plan/SPEC/code on the substance
     of the topic at this step.
  2. Start the discussion in chat (questions/options/pros-cons) — the status
     changes to `Discussing`.
  3. Based on the discussion — an explicit user decision:
     - accepted → status `Accepted`, the topic is moved into the plan as a separate
       step/edit, the number of this step is noted in the table;
     - rejected → status `Rejected`, the record stays in the table (not
       deleted, for history), implementation does not start.
  As everywhere — discussion by itself is not permission to start work,
  a separate command `-start <number>` is needed after the status has become
  `Accepted` (see below).

- **`-start <number>`** — start performing the task with this number from
  the table of the CURRENT step/plan section (the last status table shown
  in chat).
  - This is the **only** command that means permission to start
    work. Any other phrasing — "start", "do it", "execute",
    "go ahead", agreement by meaning, a plain mention of a step number without
    a command, etc. — is NOT considered permission, even if it
    seems obvious from context.
  - The number is the ROW (task) number from the last shown table, not the
    plan step number and not the TBD number.
  - If the number doesn't correspond to any task in the last shown
    table, or it's unclear which table is meant (several tables shown
    in a row) — ask, don't start work.
  - After completion — update the status of this task in the plan and explicitly
    state in chat which plan step(s) changed.

- **`-start --step <number>`** — switch to working on step `<number>`,
  WITHOUT starting any specific task:
  1. Show in chat the task table for this step from its `### Tasks` section
     (in `plan.md` for the monolith, or in the step file for the split layout).
  2. Mark this step as CURRENT — used further by default
     for `-QA` without a parameter and as the context for a bare `-start <number>`
     (the task number is now taken from THIS table, until you switch
     to another step).
  3. Implement nothing — a purely navigational command, gives no permission to
     work (same as `-tbd`/discussion). Permission for a
     specific task — still only `-start <number>` (without
     `--step`) after the table is shown.
  - If a step with such a number doesn't exist in the plan — ask, don't
    guess.

- **`-start --day`** — start a new working day / session (opposite of `-park`):
  reload context (re-read the rules file + entry point + `plan.md` + the active
  step; if the context path is unknown/inaccessible, ask for it), ensure the
  non-avoidable reminders are active, then show a short brief (what was done +
  current status). Loading/navigation only - NOT an edit-authorizing `-start`.

- **Rules about the row number in tables and about the screen/module in tasks** —
  moved to `templates/actualization-rules.md`, items 5-6. Not
  duplicated here.

- **`-commit`** (also recognize the old phrasing "create a commit",
  if it slips through out of habit somewhere) — before preparing the text
  of the commit message:
  1. Ask the user for the current `git status` / `git diff`, if the
     assistant has no working git access in this session (don't
     assume access is there — check for real every time).
  2. Prepare the commit text strictly by what actually changed (by
     the user's output), don't make it up from memory of the conversation.
  3. Message style — a short heading + if needed
     bullet points grouped by file, without unnecessary explanations, unless the project
     has explicitly specified another format.

- **`-new`** — create a new entry in the project plan: a step or a task.
  After receiving the command, FIRST ask the user for the text (of the
  task/step), and only then save it to the plan. Like `-tbd`/`-start
  --step`, `-new` does NOT authorize implementation — it only records the
  entry; performing the task is still only via `-start <number>`. After
  writing — state which step(s) changed.

  Arguments:
  - **`--t`** — a new TASK in the ACTIVE (current) step. Numbering: if the
    step has a trailing QA task, the new task is inserted BEFORE it (takes
    its number, the QA task shifts by +1 — QA always stays last);
    otherwise — the next sequential number.
  - **`--s`** — a new STEP. Ask for the number, name, and gist; in the
    monolith — add a `## Step N` section to `plan.md` (with `### Tasks`/
    `### Manual QA`); in the split layout — create `plan/step-<N>.md` and a
    row in `## Step index`.
  - **`--s <number> --t`** — a new task in step `<number>` (not the active
    one); same numbering rule; that step becomes the current context.

  When creating an entry, also estimate the needed effort (medium — the
  default; high — native interop/risky spike/major architectural fork); if
  high — note it on the task and warn that the model effort must be
  switched to high before `-start` (see `working-conventions.md`).

- **`-drift`** — check the project context files for drift (plan / step files /
  SPEC / context entry file / devHistory vs each other and vs the code); report
  the discrepancies, don't fix silently (see `templates/actualization-rules.md`).
  - **`--fix`** → fix the found drifts. If a `-drift` check wasn't just run, run
    it first, then prepare the fixes and show status. Fixes to context files
    still obey the `-start` gate (shown as diffs, applied only on `-start`).

- **`-park`** — prepare for closing (end of session):
  1. save all open context tasks;
  2. sync files (context in step with code/state);
  3. run `-drift`;
  4. show current token-economy status;
  5. if no problem found — show the day brief + a closing message.
  Context writes still obey the `-start` gate.
