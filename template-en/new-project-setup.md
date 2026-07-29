# New Project Setup — bootstrapping a new project

Prompt for the assistant (Claude). Use it as the first message in a
new chat when you need to start a new project with the context
structure worked out here (tagsExpert): `commands.md`, `working-conventions.md`,
`actualization-rules.md`, `docs/SPEC.md`, `docs/plan.md`, `docs/errors.md`,
`docs/devHistory.md`, entry point `<ProjectName>Prompt.md`. Templates for all
these files are in `templates/` — a copy of the same structure already
used in tagsExpert.

No step is performed silently — each requires a response/confirmation
from the user wherever indicated.

**First of all — read `README.md`** (purpose and working model). Key: the
template runs ONE project/version at a time through the phases **spec → plan →
implementation → verification → completion**; each phase = 1+ tasks; `plan.md`
is the plan of the current version (its phase-steps), while finished work and
past versions go to `devHistory.md`, not the plan. Steps 1-4 below ARE the
"spec" and "plan" phases.

---

## Step 1 — project interview

Ask questions (don't dump everything into one message — use the
question tool if available, or split into 2-3 messages):

1. Project name.
2. One or two sentences: what it is and why (elevator pitch).
3. Technologies/stack (language, framework, platform) — or "not decided
   yet, let's discuss".
4. Where the project will live (path on disk / repository) — new code or
   already existing.
5. Language of code/comments/commits vs. language of chat communication, if
   they differ.
6. Will there be automated tests — is `docs/tests-catalog.csv` needed.
7. Edit policy: change files right away or only after explicit
   permission for each change.
8. File encoding: preserve a specific encoding (legacy, e.g. a code page like
   Windows-1255) or is UTF-8/Unicode fine (the default edit tool may re-encode
   files).
9. Change-application pattern: does the assistant write directly to the working
   copy you build/commit from, or are code changes handed as diffs/snippets for
   manual apply (e.g. the build machine is a separate/locked-down box)? —
   determines direct-edit vs snippets, and how it verifies (its reads vs your
   git diff / test output).

Don't move on to step 2 until at least items 1-4 have been answered.

---

## Step 2 — create the folder structure and context files

```
<project-root>/
  <ProjectName>Prompt.md      <- from templates/context-file-template.md
  commands.md                 <- copy of the template, unchanged
  working-conventions.md      <- copy of the template, unchanged
  actualization-rules.md      <- copy of the template, unchanged
  docs/
    SPEC.md                    <- from templates/SPEC-template.md, sections 1/3/4
                                  pre-filled with step 1 answers, the rest TODO
    plan.md                    <- from templates/plan-template.md (without steps — steps 0+
                                  are added after the SPEC is agreed)
    errors.md                  <- from templates/errors.md, empty table
    devHistory.md              <- from templates/devHistory.md, without entries —
                                  the first entry ("Session 1") is added at the end of
                                  THIS session, once the bootstrap is finished
    README.md                  <- short entry point for the git host (optional)
    tests-catalog.csv           <- only if step 1, item 6 is "yes"
```

A new project starts with a **monolithic** `plan.md` (all steps inside it).
Once the plan grows past the threshold (see `plan-template.md`) it is split
into the **split** layout: `plan.md` = `## Step index`, each step's body in
`docs/plan/step-<N>.md` (committed ones — `docs/plan/archive/`). The split is
done later as a separate operation (see `migration.md`); do not create the
`docs/plan/` folder at bootstrap.

Before creating — show the user this list of paths and ask for
confirmation (they may want different names/location).

---

## Step 3 — offer to copy the ready-made context files

Explicitly ask the user whether to copy as-is (without substantive
edits at this step):
- `commands.md` (commands `-commands`/`-QA`/`-commit`);
- `working-conventions.md` (general working conventions);
- `actualization-rules.md` (rules for keeping documentation/artifacts up to date);
- `docs/errors.md` (empty template for the discrepancies table);
- `docs/devHistory.md` (empty session log);
- `docs/plan.md` (template with meta-rules, without steps).

Wait for confirmation, don't copy silently.

---

## Step 4 — write the project SPEC together with the user

`docs/SPEC.md` — don't fabricate content by guessing. Walk the
user through the sections of `templates/SPEC-template.md` as a structured
interview: purpose → data model → core logic → storage → (UI,
if applicable) → non-functional requirements → development environment.

Rules during the interview (from `working-conventions.md`):
- Every non-trivial design question is an explicit question with options and a
  recommendation — don't choose on your own.
- The "Open questions" section of the SPEC must not be empty at the start —
  everything not yet decided goes there; an item leaves it only when the
  user has explicitly agreed to a decision.
- The SPEC is not considered ready for development until the user has explicitly
  confirmed it (not necessarily a full "Approval" — at least a verbal
  "ok, let's go").

---

## After step 4

1. Fill in the entry point (`<ProjectName>Prompt.md`): "What it is"/"Stack"/
   "Right now" (status: "initialization, SPEC agreed, plan not yet
   written out" or similar).
2. Together with the user, build `docs/plan.md` as the plan of the CURRENT
   version — phase-steps for the agreed spec (see the model in `README.md`):
   typically implementation → verification → completion (spec/plan are already
   done in steps 1-4). Each step = 1+ tasks. Only the current version's steps
   go into the plan; finished work and past versions go to `devHistory.md`,
   not the plan. `-plan` then shows exactly these steps.
3. Add the first entry to `docs/devHistory.md` ("Session 1") — a summary
   of everything done in this bootstrap session (interview → structure →
   SPEC → first plan steps). The "Right now" section of the entry point is
   a link to this entry, not a retelling.
4. Ask whether a git repository is needed from scratch (`git init`, first commit,
   push) — see the example commands in `plan-template.md`/in the ready
   `docs/plan.md` of this repository (tagsExpert) as a reference, if needed.
