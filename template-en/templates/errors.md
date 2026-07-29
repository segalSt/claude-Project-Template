# Errors / discrepancies

Reusable template — not tied to a specific project. Tracking
discrepancies between the plan/SPEC and the actual code, found during reconciliations. This is not
an application bug tracker — it's about documentation being out of sync with reality
(the plan says one thing, the code does another).

**Rule:** any discrepancy found when reconciling the plan/SPEC against the
code/tests — is not fixed silently, but first recorded here as a single
table row. Status `Open` until fixed, `Fixed` after. Each
subsequent project edit must also verify that already-`Fixed`
records have not returned — not only introduce a new change.

| Step/section | Status | Error | Description |
|---|---|---|---|
| _(example)_ | Open | _what exactly diverged (plan vs. code, SPEC vs. UI, ...)_ | _how it was discovered, what needs fixing_ |

<!--
Example of a filled-in row (delete before real use):
| Step 2 | Fixed | Cascade delete was considered done, actually wasn't | Discovered during manual testing after step 2 — added in the same session, tests added |
-->
