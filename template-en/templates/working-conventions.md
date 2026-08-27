# General working conventions

Reusable file — not tied to a specific project, copied into
other repositories as-is. Rules and patterns of how the assistant should
work on a project, not about the project itself. The project context file
(equivalent of `tegExpertPrompt.md`) should state only differences/clarifications,
not repeat the text of these rules.

**Reference file — not required to be loaded in full every session.** The
mandatory minimum of critical gates (what actually blocks an edit/"Done"
status) is in `guardrails.md` — re-read that always. Come here as needed,
when a decision requires the detail/rationale/exception behind a specific
rule beyond what `guardrails.md` covers, not by loading this file in full
by default.

## Changes and permissions

- Changes to files — only after explicit permission from the user, if
  the project stipulated this (clarify once at the start, don't assume by
  default either "yes" or "no"). The full wording (extends to
  ANY project artifact — code, SPEC, mockups, context file) —
  `templates/actualization-rules.md`, item 7.
- **Discussion is not permission.** Breaking a task into steps, clarifying
  questions, evaluating/analyzing the request text — not permission to start
  edits — see `templates/actualization-rules.md`, rule 7. Not duplicated here.
- **No assumptions — verify against the source first (non-avoidable).** Before
  stating any factual claim about code, data, or behavior as an answer, read the
  actual source (code, XML, logs). If you need a source you don't have, **ALWAYS** ask
  the user for it; only if it is confirmed unavailable may you state the claim,
  clearly signed `[assumption:]`. Never present a guess as a verified answer.
- **The only command that means permission to start work —
  `-start <number>`** — the full mechanics (gate, row number, what
  counts/doesn't count as permission) are described in `commands.md`. Not
  duplicated here.
- Non-trivial architectural/design decisions — first an explicit question with
  options and a recommendation, wait for explicit agreement, don't implement
  and don't redo in circles (implemented → discussed again → threw away —
  costlier than asking in advance).
- **Suggested changes — always a unified diff** (same format for all changes):
  for ANY file, every time: full file name (+ method if source code); `@@ lines
  N-M @@` locates each hunk with line numbers; at least 10 lines of surrounding
  original code each side (more for tricky edits); `-` old / `+` new.
- **Verify manually-applied changes before continuing (strict).** After the user
  confirms "done", re-read the source and validate by CODE (not line numbers):
  (a) the change is correct; (b) file encoding intact (project's declared encoding);
  (c) code-documentation conventions (change-history block + line, task markers,
  version marker); (d) no existing convention missed. Don't continue until all pass.
- **The rule of re-reading context after compaction** (mandatory, not a
  recommendation) — moved to `templates/actualization-rules.md`, item 4.
  Not duplicated here.

## Git and commits

- There may be no working shell/git in a particular session — don't
  assume access has appeared in a new session, check for real
  every time.
- Before preparing a commit message — ask the user for the current
  `git status`/`git diff`, if there's no direct access; don't make up a list
  of changes from memory of the conversation.
- Commit message — a short heading + if needed
  bullet points grouped by file, without unnecessary explanations, unless the project
  has explicitly specified another format.
- See also the `-commit` command in `commands.md`.

## "Living" work plan and actualization of artifacts

- The rules about mandatory plan synchronization, the discrepancy log
  (`errors.md`), the step reference when discussing, the test catalog and
  proactively reporting outdated data/files (with the question
  "actualize now or as a task at the end of the step") — a single file
  **`templates/actualization-rules.md`**. Not duplicated here.
- On any plan change — explicitly state in chat which step(s)
  changed (number and name), right in the same response where the edit was
  made — don't leave the user guessing where the change went.
- After changing test data/logic — remind to run the tests and
  send the output, if there's no working access to the test runner.

## Tool limitations (mine, not the project's)

- No file-deletion tool in a typical session — if a file needs to be
  removed from the repository, mark it `DEPRECATED` in the comment/header of the file
  and explicitly ask the user to delete it manually, don't leave it
  untracked somewhere in the memory of the conversation.
- No rename/move tool — "moving" a file in
  practice means copying the content to a new place (Read+Write); the
  original remains and also requires manual deletion by the user.
- If there's no working shell — unable to run build/tests/git myself;
  ask the user to run the command and send the output, don't imitate
  the result.
- The default edit/write tool may re-encode a file to UTF-8. If the project
  declared a required encoding (see the setup/migration interview) — edit such
  files only via an encoding-safe method (or hand the user snippets) and verify
  encoding after.
- If the project declared at setup that the assistant's file-write location
  differs from the working copy it's built/committed from (separate build
  machine, sandbox, etc.), hand ALL code changes as diffs/snippets for manual
  application and verify via the user's git diff / test output, not the
  assistant's own reads. (Default assumption: direct edits are fine.)

## Visual materials

- Show mockups/screen sketches one at a time (one render at a time),
  wait for the user's reaction, don't post several at once in one
  response.

## Model effort level

- When creating a task/step (`-new`) and when composing/estimating the
  plan — estimate the needed effort: **medium** — the default (ordinary
  screens, code-behind, logic + tests, docs, small fixes); **high** —
  native interop, risky spikes, major architectural forks (several
  options / a big decision).
- Mark high tasks/steps in the plan (e.g. an "effort level" section).
- If the task being started is rated high — BEFORE doing it, explicitly
  ask the user to switch the model effort to high; do not start a
  high-effort task on medium.
- The assistant does NOT switch effort itself — no tool, no access; only
  the user can change the level in the client. So "ask to switch" is the
  only path.

## Manual QA by checklist

- A manual QA pass over a checklist (e.g. per screen) — ask strictly ONE
  item at a time, not as a list (a question with options). Each question
  MUST contain: (1) what is being checked; (2) step-by-step reproduction
  instructions in the app; (3) the expected result. Options: "Passed" /
  "Failed" / "Skip". Do not move to the next item without an answer on the
  current one.
- Passed items — keep a running list in chat. Failed items (app bugs) —
  record immediately in the project's QA findings file (e.g.
  `docs/qa-findings.md`): id, screen, symptom, expected behavior, status
  Open/Fixed. NOT in `errors.md` (that one is about docs-vs-code drift, not
  app bugs).
- On checklist completion — a summary (passed/failed/skipped), then walk
  the findings file: for each Open bug ask the user which plan step to log
  it as a task under, and run `-new` (the "task in step" form); the task
  text comes from the bug description.
- After a bug is fixed — mark it Fixed in the findings file.

## Token economy

Moved to **`templates/token-economy.md`** (strict) — output brevity, file-size
discipline, model right-sizing, sub-agent delegation, session-length. Not
duplicated here; update it there.

## Language

- Explicitly state (in the project context file) the language of code/comments/
  commit messages and separately — the language of communication with the user in chat, if
  they differ. Don't confuse these two language contexts.
