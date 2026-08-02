# Token economy (strict)

Reusable rules for low context/token usage. The session-rules file and the
working-conventions reference here — don't duplicate the text, update it here.

## Output
- **English by default** (chat, doc files, code comments) unless the user says
  otherwise — densest/cheapest language.
- Skip meaningless preambles/afterwords: no "I'll now…", "let me check…",
  "My honest…", "Good catch", no restating what was just read, no step-by-step reports.
- Wrap-up = at most **one** sentence, with a short explanation.
- Direct answers/code; don't reprint full unmodified files; don't re-read a file
  just edited.
- Simple language, no jargon; concise, not cryptic.

## Context & files
- Keep often-read/updated context files (session rules, plan, history, entry
  point) short — soft ceiling ~150-200 lines; detail lives in step files/code.
- One source per fact — reference, don't duplicate.
- Prefer Markdown for documents; produce formatted deliverables (Word/PDF) only
  when required.

## Model, delegation & session length
- **Right-size the model/effort** to the task: the lightest that fits; escalate
  to a larger model / higher effort only for hard work (native interop, big
  architectural forks). Confirm the suitable model at the start of a task.
- Delegate heavy reads/audits (drift scans, characterization, large-file
  surveys) to **sub-agents** in isolated context — keep the main window clean,
  take back only conclusions.
- At **~100k tokens** (or when context clearly grows large — the exact count may
  not be visible to me), **warn the user and suggest `-park` + a fresh session**.
