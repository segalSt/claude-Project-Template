# Development history (devHistory)

Reusable template. A chronological log of development sessions — what
changed since the previous session (a delta in human language), not a duplicate of
the plan/SPEC (those live in `SPEC.md`/`plan.md`). The most recent entry —
at the top, added at the end of each session. The project context file
(equivalent of `tegExpertPrompt.md`) does not retell the delta in the "Right
now" section — it only references the most recent entry here + keeps the
minimum critical for continuing (what is not committed, what's next).

**When to split into files (scalable layout, same principle as
`plan-template.md`).** While the log is small, all sessions live directly
in this file. When reading the whole file becomes expensive (a guideline,
not a rule — split when `devHistory.md` exceeds ~300-400 lines, or there
are more than ~6-8 sessions) — split the layout:
- `devHistory.md` keeps: **`## Session index`** (table: session №, date,
  one-line summary, link to file) + **the full text of the LATEST
  session, inline**, without opening an extra file;
- older sessions are moved VERBATIM (without changing content) to
  `history/session-<N>.md`, one file per session.

Moving is a mechanical operation — don't rewrite the meaning during the
move. The project context file keeps referencing here (index + latest
session), not the `history/` files.

---

## Session 1 — <date>

- _(a summary of this session's changes, point by point — what was done, what was
  discovered, what decisions were made)_

**Not committed at end of session:** _(if any — what exactly)_

---

<!-- While the layout is monolithic: add "## Session N — <date>" at the
     top, the newest entry — right under this template comment, previous
     ones shift down. After splitting (see above), this file keeps only
     the latest session in full + the index of all sessions; previous
     full texts live in history/session-<N>.md. -->
