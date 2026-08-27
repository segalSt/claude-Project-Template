# Quick reference: hard rules (guardrails)

Reusable file — not tied to a specific project, copied as-is. Re-read this
file FIRST at the start of every session and after context compaction
(compaction/restart) — it's a cheap digest of the rules that actually BLOCK
an action in practice. Full versions with rationale and detail live in
`working-conventions.md` and `actualization-rules.md`; if interpretations
conflict, those are authoritative — this file is only the condensed version
for cheap loading into context.

1. **File edits — only via `-start <number>`.** The only command that means
   permission to start work. Any other phrasing ("go ahead", "do it", "ok",
   agreement by meaning, mentioning a step number without the command) is
   NOT permission, even if it seems obvious from the conversation. An edit
   is allowed only when the immediately preceding user message is exactly
   `-start`/`-start <number>`. Details — `commands.md`.
2. **After compaction/session restart — re-read context first.** The
   project's entry point + `plan.md` + this file — BEFORE any other action
   and BEFORE the first response to the user, including a direct question or
   a short clarification. An auto-generated session summary is not a source
   of truth, even when it itself suggests "continue implementation".
   Details — `actualization-rules.md`, rule 4.
3. **"Done" — only after confirmed build + automated tests + (if there's UI)
   a visual check.** Before that it's "Implemented, awaiting check"; "Done,
   confirmed" — only after the user explicitly confirms. Details —
   `actualization-rules.md`, rule 10.
4. **Discussion/analysis/assessment is never itself permission to act.**
   Neither `-tbd`, nor `-new`, nor breaking a task into steps, nor answering
   a clarifying question grants permission to edit by itself — permission
   comes only from rule 1 above.
5. **A discrepancy — report it explicitly, don't fix it silently and don't
   ignore it.** A found drift (plan/SPEC/code/mockup/test catalog) — say in
   chat exactly what's stale and in which file, then ask whether to
   actualize now or file it as a separate task. Details —
   `actualization-rules.md`, rule 9.
6. **Without checking the source — don't assert.** Before stating anything
   about code/data/behavior as an answer, read the real source (code, XML,
   logs). If a needed source doesn't exist — ask the user, or explicitly
   flag it `[assumption:]`; never pass off a guess as a verified answer.
   Details — `working-conventions.md`.

Beyond this — as needed, not on every load: token economy
(`token-economy.md`), effort level and the rest of the conventions
(`working-conventions.md`), the full list of actualization rules with
rationale (`actualization-rules.md`).
