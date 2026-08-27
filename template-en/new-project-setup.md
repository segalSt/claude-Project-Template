# New Project Setup — bootstrapping a new project

Prompt for the assistant (Claude). Use it as the first message in a
new chat when you need to start a new project with the context
structure worked out here (tagsExpert): `commands.md`, `guardrails.md`,
`working-conventions.md`, `actualization-rules.md`, `docs/SPEC.md`, `docs/plan.md`, `docs/errors.md`,
`docs/qa-findings.md`, `docs/devHistory.md`, entry point
`<ProjectName>Prompt.md`. Templates for all
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

## Step -1 — is this too big for one session?

This scaffold (Steps 0-4) assumes the project/version is small enough that
its spec fits in one session — grill it, write `docs/SPEC.md`, done. If the
idea is instead large and foggy (spans multiple subsystems, a long-running
multi-quarter effort, many cross-cutting decisions not yet visible), that
assumption breaks: use `/wayfinder` (mattpocock skill) first instead. It
charts a map of decision tickets on the issue tracker (needs
`/setup-matt-pocock-skills` run first) and resolves them one at a time —
each "grilling" ticket on that map calls the same two skills
`/grill-with-docs` does, one level down. Come back to Step 0 here once a
ticket (or the whole map, if it turns out small) narrows down to a single
project/version-sized scope.

If it's unclear which applies, ask the user rather than guessing; a
project this small in scope defaults to skipping straight to Step 0.

---

## Step 0 — check for prior `/grill-with-docs` output, or offer to run it

Before interviewing, check whether `CONTEXT.md` and/or `docs/adr/` already
exist at the project root.

**If they exist** (e.g. from a `/grill-with-docs` session run before this
bootstrap), they are a **source of answers**, not something to ignore in
favor of a fresh interview:

- Read them fully.
- In Step 1, skip any question already answered there (elevator pitch,
  stack, data model/terminology, non-functional constraints, etc.) — state
  what you found instead of asking. Only ask about items 1-9 below that are
  genuinely still unanswered.
- In Step 4, use them as the source for drafting `docs/SPEC.md` (see Step 4).

**If they don't exist**, actively offer to run `/grill-with-docs` first,
before Step 1 — recommend it for any project with real design/domain
questions to work out; a truly trivial project can skip straight to the
interview if the user prefers. Wait for the user's choice, don't assume.
If they accept, run it, then treat its output per the paragraph above before
continuing to Step 1.

This does not replace Step 1-4 — CONTEXT.md/ADRs feed `docs/SPEC.md` here,
the same file, same template, same location. It's a separate matter from
`/to-spec` (a different mattpocock skill): that one is for individual
features/tickets once implementation is underway, and publishes to this
repo's issue tracker in its own template — it does not write `docs/SPEC.md`
and is not part of bootstrapping a new project. Don't invoke it here.

---

## Step 1 — project interview

Ask questions (don't dump everything into one message — use the
question tool if available, or split into 2-3 messages). Skip any item
already answered by `CONTEXT.md`/`docs/adr/` per Step 0 — state the answer
you found instead of asking:

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
  guardrails.md               <- copy of the template, unchanged
  working-conventions.md      <- copy of the template, unchanged
  token-economy.md            <- copy of the template, unchanged
  actualization-rules.md      <- copy of the template, unchanged
  docs/
    SPEC.md                    <- from templates/SPEC-template.md, sections 1/3/4
                                  pre-filled with step 1 answers, the rest TODO
    plan.md                    <- from templates/plan-template.md (without steps — steps 0+
                                  are added after the SPEC is agreed)
    errors.md                  <- from templates/errors.md, empty table
    qa-findings.md             <- from templates/qa-findings.md, empty table
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
- `guardrails.md` (quick reference — hard rules, read first every session
  and after context compaction);
- `working-conventions.md` (general working conventions);
- `token-economy.md` (token-economy rules);
- `actualization-rules.md` (rules for keeping documentation/artifacts up to date);
- `docs/errors.md` (empty template for the discrepancies table);
- `docs/qa-findings.md` (empty template for the QA-findings table);
- `docs/devHistory.md` (empty session log);
- `docs/plan.md` (template with meta-rules, without steps).

Wait for confirmation, don't copy silently.

---

## Step 4 — write the project SPEC together with the user

`docs/SPEC.md` — don't fabricate content by guessing.

- **If Step 0 found `CONTEXT.md`/`docs/adr/`**: draft the sections of
  `templates/SPEC-template.md` directly from them instead of interviewing
  from scratch — Purpose from CONTEXT.md's opening description, Data model
  from its Language/glossary terms, Core logic and Non-functional
  requirements from the resolved decisions and ADRs, Development environment
  from any stack-related ADR. Show the user the full draft, section by
  section, and get explicit confirmation on each — same bar as an interview
  answer, just starting from a draft instead of a blank section. Anything
  the grill session left open (or that doesn't map cleanly to a SPEC
  section) still goes through a real question with options and a
  recommendation, same as below.
- **Otherwise**: walk the user through the sections of
  `templates/SPEC-template.md` as a structured interview: purpose → data
  model → core logic → storage → (UI, if applicable) → non-functional
  requirements → development environment.

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
5. Offer to run `/setup-matt-pocock-skills` now, while still in this
   bootstrap session — it configures this repo's issue tracker (GitHub/
   GitLab/local `.scratch/`) and triage labels, a one-time setup. Getting it
   done here means it's ready before the first feature/bug comes up. Not
   required to finish bootstrapping; the user can defer it.

Once implementation is underway and individual features/bugs come up as
separate units of work, `/to-spec` (mattpocock skill) is the tool for those —
it synthesizes and publishes a spec for that one increment to the issue
tracker configured above. It's a parallel, later-stage tool for per-ticket
specs, not a substitute for `docs/SPEC.md` above — don't invoke it during
bootstrap.

---

## Step 5 — cleanup: move into `context/`, delete the bootstrap tooling

Once the bootstrap is genuinely done (Steps 1-4 + "After step 4" items 1-5
all complete — SPEC agreed, plan built, entry point filled, `devHistory.md`
has its Session 1, git initialized if wanted, `/setup-matt-pocock-skills`
run if wanted), the project no longer needs its own local copy of the
bootstrapping tooling, and its working files don't need to sit loose at
project root. **Copy-then-delete, never delete-then-copy** — verify every
target file exists and reads correctly before removing its source.

1. Create `context/` at the project root.
2. Move into `context/` (a straight rename/move, content unchanged):
   `commands.md`, `guardrails.md`, `working-conventions.md`, `token-economy.md`,
   `actualization-rules.md`, `CONTEXT.md` (if `/grill-with-docs` was used),
   this toolkit's own `README.md` (the "purpose and working model" file —
   distinct from any project-specific `docs/README.md` for a git host,
   which moves too but keeps its own name), and the whole `docs/` folder
   (so `docs/SPEC.md` → `context/docs/SPEC.md`, `docs/adr/` →
   `context/docs/adr/`, `docs/agents/` (if `/setup-matt-pocock-skills` ran)
   → `context/docs/agents/`, etc. — everything under `docs/` moves as a
   unit).
3. Once every moved file is confirmed present under `context/` — delete
   `new-project-setup.md`, `migration.md`, and `templates/` (the whole
   folder) from the project root. These are the bootstrapping MECHANISM,
   not the project's own content; once bootstrap is done they're spent.
4. **Actualize every path reference** in every file that remains (the entry
   point, and everything now under `context/`) so they point at the new
   `context/`-relative locations instead of the old root-relative ones —
   the same discipline as `actualization-rules.md` rule 9, done once,
   mechanically, as the last act of bootstrap. Don't leave dangling
   references to a `templates/` or root-level file that no longer exists.
5. The project root ends up holding only the entry point
   (`<ProjectName>Prompt.md`) as reusable-template content — plus whatever
   the project's own code/build tooling needs there, and any file a
   *different* system requires at root regardless of this template (e.g.
   `CLAUDE.md`/`AGENTS.md` for `/setup-matt-pocock-skills`, which stays at
   root by that tool's own convention, not this one's).

This step is optional but recommended once bootstrap is truly finished;
it can also be done later, separately (it isn't tied to happening in the
same session as Steps 1-4).
