# QA findings (application bugs)

Reusable template — not tied to a specific project, copied into other
repositories as-is. Application BUGS found during a manual QA run against
checklists (the "Manual QA" section of a plan step). Unlike `errors.md`
(documentation out of sync with code), this tracks real behavioral defects
in the application.

**Process** (rule — `working-conventions.md`, "Manual QA checklist run"
section):
- During the run, a failed item → a row here, status `Open`.
- After finishing the checklist — for each `Open` bug, ask the user which
  plan step to file it under as a task, and run `-new` (task in a step);
  the task text comes from the bug description.
- After the bug is fixed → status `Fixed` (with a link to the step/task).

| id | Screen | Symptom | Expected behavior | Status | Step/task |
|---|---|---|---|---|---|
| _(example)_ | _(screen/window)_ | _(what goes wrong)_ | _(what should happen)_ | Open | _(-)_ |

<!--
Example of a filled-in row (delete before real use):
| 1 | Tag management | After closing the card, tree focus always moves to the parent node | Focus stays on the current node | Fixed | Step 2.65, task 1 |
-->
