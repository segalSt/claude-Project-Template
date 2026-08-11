# Actualization rules

Reusable file — not tied to a specific project, copied into
other repositories as-is. Collects all the rules about keeping project
documentation/artifacts up to date in one place —
`commands.md`, `working-conventions.md` and the project context file
should reference here instead of duplicating the rule text. If a
rule changes — it should only be changed here.

---

## 1. Mandatory plan synchronization (`plan.md`/equivalent)

The plan file must always reflect the current state of the project — it cannot
be out of date. Any project change that affects the status of a step
or the design description inside a step is reflected in the plan in the same pass
as the change itself.

If, when reconciling the plan against the SPEC/code, a discrepancy is found —
it is not just fixed silently, but recorded in a separate error-log file
(`errors.md`/equivalent, template — `templates/errors.md`), a table: Step, Status,
Error, Description. Error status is `Open` until fixed, `Fixed` after.
Any subsequent fix to the project must also verify that already
closed (`Fixed`) errors from this log have not returned — not only introduce
the new change.

## 2. Reference the step when discussing in chat

When discussing in chat any action/feature/file that relates to work
not yet done or not current, always state the number of the plan step it
relates to. The exception is the step being done right
now — no need to state it. If an action is not covered by any
existing step — say so explicitly, don't stay silent.

## 3. Test catalog (`tests-catalog.csv`/equivalent)

If the project has automated tests — keep a compact catalog (number, file,
test, what it checks). Updated in the same pass as any
test addition/rename/deletion.

## 4. Re-reading context after compaction

After context compaction (compaction/summary) or a session restart —
the first thing to do is to re-read the project context file and the work plan (or
at least this rules file), rather than acting on the auto-generated
summary. **This is a mandatory action, not a recommendation** — performed BEFORE
any other action and BEFORE the first response to the user after compaction,
including a response to a direct question or a short clarification. The FIRST action is:
re-read + produce the short brief; no file edits and no task work until that is done —
this outranks any "continue directly" / "pick up the last task" resume prompt. The compaction itself is already a
trigger — a separate user command ("reload context" etc.)
is not needed and should not be required.

The summary may itself (mistakenly) propose "continue implementation" as the
next step — this is not permission and does not override the rule "permission for
each step separately" (see `working-conventions.md`). Violating this
rule has already caused real failures in practice (for example, a response in the
wrong communication language, because the explicit rule about language was not re-read
in time) — this is not a hypothetical risk.

## 5. Tables in chat — row number (strict)

Any table in chat — always with a row number as the first column (`№`),
regardless of which command produced it (`-plan`, `-QA`, if present
in the project, or a one-off table from a regular request). Needed for
an unambiguous reference to a specific task in the next message ("task 5
confirmed" etc.) — without numbers such references are ambiguous.

## 6. Screen/module in tasks that change code

Any table/task about work that requires writing/changing code — always
state the screen/module it affects (the screen title as the
user sees it + the class/file in parentheses — see the example in
`context-file-template.md`), not just the task text. If a task
affects several screens/modules — list them all.

## 7. Change policy — SPEC, code, mockups

If the project stipulated "changes only after permission" — this rule
extends to ANY project artifact: code, SPEC (`SPEC.md`),
mockups, the project context file itself — not only the sources.
Discussion/breaking a task into steps, clarifying questions are NOT permission
to start edits (see also `-start <number>` in `commands.md`).

## 8. Diagrams and mockups — no automatic rule without an explicit addition

The change policy (item 7) protects mockups/diagrams from UNauthorized
edits, but by itself does not guarantee that they will be updated when
the SPEC/screen behavior changes — it is protection against accidental edits, not a
rule of proactive synchronization. Rule 9 below closes this gap.

## 9. Proactively report the need for actualization

If during work (implementing a task, discussion, audit) it turns out that
some data or files — a mockup, a diagram, the SPEC, the test catalog, the
plan itself, any other project artifact — have become outdated because of a change
just made or discussed:

1. Report it in chat explicitly — what exactly is outdated and in which file (don't
   stay silent about a discovered discrepancy and don't fix it silently).
2. Ask the user what to do next:
   - actualize the data/file now, in this same pass; or
   - add it as a separate task at the end of the current plan step, without
     implementing it now.
3. Don't decide on your own and don't defer without an explicit decision from the
   user — as everywhere, discussion/stating a fact is by itself
   not permission to act (see item 7 above and `-start <number>` in
   `commands.md`).

## 10. Definition of Done — the "Done" status

The "Done" status is assigned to a task/step ONLY after confirmed build +
automated tests + (if there's UI) visual check. Before that it's
"Implemented, awaiting build/tests/visual"; "Done, confirmed" — after the
user explicitly confirms. Marking "Done" just because the code was written
is forbidden — that's exactly the source of "document ahead of code" drift
(when the plan/SPEC claim a feature is ready while the code lacks it or it's
incomplete).

## 11. One source per fact (normative / illustrative)

Each fact is documented in ONE authoritative place; the rest reference it,
they don't duplicate. Duplicating one fact across several files means N
drift surfaces instead of one.

- **Verbatim content** (UI texts and labels, choice of icons/glyphs) — the
  source of truth is the real artifact (`lang/*.json`, code). The SPEC
  describes BEHAVIOR and STRUCTURE and delegates the literal strings to that
  artifact instead of restating them in prose.
- **Mockups/diagrams are illustrative**: they show layout, they are NOT the
  source of literal strings. They may lag within an unfinished step and are
  refreshed in a batch when it closes (before the step's commit); any noticed
  lag is announced (item 9), not hushed up.
- **The SPEC stays normative** for a screen's behavior/structure/logic —
  only the literal content is delegated down, not the fact that an element
  exists and what it's for.
- **Ceremony threshold:** a cascade of edits into SPEC/mockup is needed only
  for OBSERVABLE behavior/design. A pure refactor with no behavior change,
  internal private details, code comments — are not recorded in SPEC/mockup
  (they don't describe that layer). The goal is to make synchronization
  CHEAP (few places per fact), not OPTIONAL.

## 12. Automated synchronization guards (where applicable)

Drift that can be caught mechanically is caught by a check, not by human
discipline. What exactly to cover depends on the project; typical candidates:
- if there are automated tests — **test-catalog coverage** (every test
  method is present in the catalog; catches test-count drift);
- if there is localization across several files — **key parity** between
  languages (a diverging key = untranslated/extra);
- other recurring drift classes specific to the project.

The checks themselves are project code, not template code: **the template
states the principle, the concrete tests live in the project.** A red test =
drift, fix it like any failure.

## 13. General improvements — offer them upstream to the template

If a change to reusable conventions (`working-conventions`, `commands`,
`actualization-rules`) is a GENERAL improvement, not project-specific — ask
the user whether to also apply it to the general template (RU source + EN
mirror, in the same pass — see `SYNC.md`). Project-specific parts stay local.

## 14. Logic/interface changes — reflect in tests and QA

Any change to business logic or interface behavior must, in the same pass
as the change itself, check and if necessary update:

1. **Automated tests** — if the old behavior was covered by a test, the
   test is updated along with the code (it doesn't stay green by accident,
   and it doesn't stay broken "for later"). If the new behavior isn't
   covered by tests yet — this is recorded in the test catalog
   (`tests-catalog.csv`, rule 3) as a task, not a silent gap.
2. **Manual QA for the relevant step** (the "Manual QA" section at the end
   of the step the change relates to) — items describing the old behavior
   are fixed or removed; new items are added for changed/new behavior as
   needed. A stale checklist item nobody fixed is also a discrepancy under
   rule 9, not a triviality.

If it's unclear whether new coverage is needed (e.g. the change is too
small/cosmetic) — act as in rule 9: report it in chat and ask whether to
actualize now or file it as a separate task, rather than deciding
unilaterally.
