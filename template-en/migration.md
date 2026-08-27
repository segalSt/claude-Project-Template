# Migrating an existing project to the template structure

Reusable file. Describes how to bring an ALREADY existing project (with
ad-hoc docs or on an old layout) to the current template structure. Three
scenarios:

- **A. Adopt** — a project with no template structure at all → create the
  context files (`commands.md`, `guardrails.md`, `working-conventions.md`,
  `actualization-rules.md`, `docs/SPEC.md`, `docs/plan.md`, `errors.md`,
  `devHistory.md`, entry point).
- **B. Split** — a project already on the template, but with a monolithic
  `plan.md` that outgrew the threshold (see `plan-template.md`) → move steps
  into files (`plan/step-<N>.md` + `plan/archive/`), and reduce `plan.md` to
  a `## Step index`.
- **C. Reconcile** — the project ALREADY has its own mature structure/rules (a
  different convention) → make the template rules authoritative and **adapt**:
  see the principle below.

**PRINCIPLE (key): migration is ADAPTATION, not dumb copying.** The template
rules become authoritative. Outdated project rules are **replaced** by the new
ones (old commands → the template's commands; outdated numbering/layout → the
new one). The project's domain content (architecture, code-change rules,
tasks, diagrams) is **preserved** and re-expressed in the new structure.
Copying an old file with outdated rules verbatim (e.g. an old command table) is
NOT migration and counts as an error — it leaves old and new rules in conflict.

**Target model (see `README.md`).** The result must fit the template's model:
ONE project/version at a time through the phases spec → plan → implementation →
verification → completion; each phase = 1+ tasks. So `docs/plan.md` = the
phase-steps of the CURRENT version, NOT a registry of different tasks and NOT a
history. Finished/past tasks and versions move to `docs/devHistory.md`
(history), they do NOT go into the plan. Common mistake (seen on a real
migration): collapsing independent past tasks into plan rows — then `-plan`
shows history instead of the current version's action plan.

No step is performed silently — each requires user confirmation where
indicated (same policy as `new-project-setup.md`).

---

## Interactive process — the `-migrate` command

Migration is triggered by the **`-migrate`** command and runs as a
dialogue:

1. **Invocation.** The user types `-migrate`.
2. **Input.** The assistant asks: (a) the path to the target project folder;
   (b) its main/entry file or where the docs live; (c) the language of the
   target structure — **RU** (`templates/`) or **EN** (`template-en/`).
3. **Analyze the old project's context.** Read the main file + `docs/` (or
   whatever exists); determine what's already present (SPEC? plan? commands?
   rules? tests/catalog? log/history?), the plan layout, the volume, what maps
   onto the template files, what's missing, and — importantly — which project
   rules are OUTDATED vs the template (e.g. its own old command system). From
   this — the scenario: **A** (no structure), **B** (monolithic plan outgrew
   the threshold), **C** (own mature structure/rules → adapt), or a combination.
4. **Clarifying questions** (if needed, in one batch via the question tool):
   language of code/comments/commits vs chat; edit policy; file encoding (keep
   a legacy encoding, e.g. Windows-1255, or UTF-8); change-application pattern
   (direct write vs snippets for manual apply); whether there are
   automated tests and whether `tests-catalog.csv` is needed; where to map
   specific existing documents; whether a split is needed right now (threshold).
   As everywhere — discussion/answers ≠ permission; wait for an explicit "go".
5. **Migration (adaptation, not copying).** If there are no further questions —
   bring the old context to the chosen-language template structure:
   preparations (below) → create/move files (A) and/or split steps (B) and/or
   **replace outdated project rules with the template's, preserving domain
   content** (C) → update `commands.md`/the context file. The mechanical move
   of large unchanged blocks is preferably done by a script (gotchas below). Do
   NOT leave verbatim copies of old rules that conflict with the new ones.
6. **Report.** Produce statistics and a description of the resulting context:
   - a list of created/moved files;
   - how many steps/tasks were distributed, the final layout (monolith/split);
   - what remains `Open`/TODO (errors/QA findings), the green-test count
     before/after, a link to the backup;
   - a short prose description of the new context (what lives where, where to
     start reading).

The sections below are the reference detail used by steps 3 and 5.

---

## Preparations (both scenarios)

1. **Clean git.** Make sure the working tree is committed (or explicitly
   ready for edits): migration is a large mechanical change, rollback must be
   cheap (`git checkout -- <file>`). If there's no git — make a manual copy
   of the affected files.
2. **Green baseline.** If there are automated tests — run them
   (`dotnet test` or equivalent) BEFORE migration, record the green count.
   Migrating docs must not change that number.
3. **Inventory.** List the project's existing documents and map them to the
   target template files (what to reuse as-is, what to move, what to create
   from scratch). Show the user, wait for "ok".
4. **Language.** Fix the language of code/comments/commits and of chat
   (template files are copied as-is; project specifics go in the context
   file). If the project is bilingual per template — keep the RU→EN sync
   (`SYNC.md`).

---

## Scenario A — adopt (introduce the template structure)

1. Run the interview and create the files as in `new-project-setup.md`,
   Steps 1-4, but NOT from scratch — absorb what's already written: existing
   README/notes → `SPEC.md`/entry point; known tasks → `plan.md` steps.
2. Start with a monolithic `plan.md` (all steps inside). Split only once it
   outgrows the threshold (scenario B), not earlier.
3. The first `devHistory.md` entry — a recap of what was already done in the
   project BEFORE the structure was introduced (reconstructed from git
   history / the user's memory), so the log doesn't start empty.

---

## Scenario B — split (monolith → per-step)

The split is a mechanical operation. **Better done by a script/shell than by
hand-retyping** (retyping hundreds of lines risks silent corruption; see the
DoD rule in `actualization-rules.md` — don't confirm without verification).

### Target layout
- `plan.md` = meta-rules + `## Step index` (№, name, STEP-LEVEL status, link)
  + cross-cutting sections (TBD/Errors/Future/Out-of-scope/Git).
- `plan/step-<N>.md` (active) and `plan/archive/step-<N>.md` (committed):
  heading + `### Tasks` (the table — the single source of task statuses,
  rule 11) + design + `### Manual QA`.
- Rows of the "## Task overview by step" master table move into the `### Tasks`
  of the respective steps; the master table in `plan.md` is replaced by the index.

### Order
1. **Back up** `plan.md` (git and/or a `.bak` copy).
2. For each step, create a file: step body + `### Tasks` (its rows from the
   master table) + `### Manual QA`.
3. **A large closed step need not be copied verbatim.** If a committed step's
   body is hundreds of lines of implementation history: put `### Tasks` + a
   short intro + a pointer that the detailed history is in git. Saves volume
   and removes the hand-corruption risk.
4. Rebuild `plan.md`: header + effort + `## Step index` (with links) + tail.
   Remove the master table.
5. Update `commands.md`/the project context file for the split (sources = the
   step files; see the edits in the template's `commands.md`).
6. **Verify with `git diff`:** a clean move = lines left `plan.md`, identical
   ones arrived in the step files. Tests (if any) — same green count as before.

### Commands (PowerShell) and gotchas (proven in practice)
The script slices `plan.md` by step headings into files and rebuilds
`plan.md`. Key lessons without which it fails:

- **Keep the script pure ASCII.** Windows PowerShell 5.1 reads `.ps1` as ANSI
  and corrupts Cyrillic in STRING literals (the parser fails before running).
  Do NOT write the Cyrillic in the step-heading pattern (`## <Shag>`) as a
  literal and NOT via `\u` (editors often expand escapes); BUILD it at runtime
  from char codes: `-join ([char]0x0428,[char]0x0430,[char]0x0433)` = "Shag".
  Or detect a step structurally without Cyrillic: `^## \S+ \d` ("## <word> <digit>").
- **Safety guard.** If suspiciously few step blocks are found — do NOT rewrite
  `plan.md`, fail with an error instead (otherwise you truncate the plan to nothing).
- **Read the source from the backup.** Read `plan.md.bak` (full), not the
  current file — then the script is idempotent even after a partial run.
- **The file may be memory-mapped** ("The requested operation cannot be
  performed on a file with a user-mapped section open") — in-place overwrite
  fails. Workaround: rename the original aside, then create a fresh file
  (creating a new file is not blocked by the old mapping).
- **Output encoding** — write files as UTF-8 without BOM
  (`New-Object System.Text.UTF8Encoding($false)`), preserve the source's line
  ending.

A working sample of such a script is in the `tagsExpert` project history
(`tools/split-plan.ps1`); when the assistant has no working shell, the user
runs the script and checks the result with `git diff`.

---

## Scenario C — reconcile (project has its own mature structure/rules)

The project is already organized, but by a DIFFERENT convention (own rule
files, own command system, own plan/task format). Goal — make the template
rules authoritative and adapt, without breaking what works and without leaving
old cruft.

1. **Map.** Build a table "project file/rule → template file/rule": what moves
   as domain content, what gets replaced, what's missing. Show the user.
2. **Replace the outdated.** Rules that have a newer version in the template —
   REPLACE, don't duplicate:
   - **commands** — replace the old command system with the template grammar
     (`commands.md`), resolving token collisions explicitly (e.g. a foreign
     `-start` ↔ the template's `-start <number>`); keep project-useful commands
     with no equivalent (e.g. `-stop`) as an addition;
   - **plan layout/numbering** — to the template's (index + step files if the
     volume warrants it; see the threshold in `plan-template.md`);
   - **actualization/DoD/one-source rules** — from the template.
3. **Preserve domain content.** Architecture, code-change rules, tasks,
   diagrams — carried into the new structure (`SPEC.md`,
   `working-conventions.md`, `plan/step-*`, `docs/diagrams/`) without losing meaning.
4. **One source.** Don't leave one fact in two places (e.g. meta-rules both in
   the old file and in `working-conventions.md`) — the old file becomes a thin
   pointer, or its rule is replaced by a reference.
5. **Tests/guards.** If there are no tests (e.g. legacy) — design a guard for
   the project (characterization tests, etc.); don't drop the auto-guard
   principle (rule 12/13).
6. **Independence (if required).** If the old context will be deleted — copy
   everything needed into the new one (deep docs, structure, diagrams, source
   meta-files), rewrite references to local paths, and BEFORE deletion
   explicitly list what is NOT carried over and will be lost (skills, reference
   sheets, etc.), letting the user rescue what matters.

Typical scenario-C mistake: copy the old rules file verbatim and add the new
one beside it — the old commands/rules stay and conflict. Always REPLACE +
preserve domain content, never layer.

---

## After migration
- Update the project context file: state the current layout (split) and where
  things live.
- A `devHistory.md` entry about the migration (what changed and why).
- `-commit` strictly by the real `git diff`.
- For Scenario A (adopt): the same "Step 5 — cleanup" from
  `new-project-setup.md` applies — move context files into `context/`,
  delete `new-project-setup.md`/`migration.md`/`templates/` from the
  project root, actualize paths. For B/C: same idea (consolidate context
  files into `context/`) if the project isn't already laid out that way.
