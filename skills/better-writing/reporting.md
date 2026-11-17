# Reporting

The severity ladder and format for a standalone review. `better-interface` replaces both when it orchestrates.

**Severity.** `HIGH` misleads the user or hides how to recover from an error. An error that names no way to recover is always `HIGH`, as is a destructive action with neither confirmation nor undo. `MEDIUM` makes copy harder to act on or to translate, or breaks voice, terminology or capitalization consistency. `LOW` is isolated wording polish.

**Format.** Group findings under the principle each violates, ordered by severity, one row per root cause listing every location it appears in:

| Severity | Location | Before | After | Why |
| --- | --- | --- | --- | --- |

`Location` is `path/to/file:line`. `Why` names the principle and the user impact.

End with `Block` when any `HIGH` remains, `Approve` otherwise, leaving the rest in the table as work to do. Never `Approve` coverage you did not inspect. With nothing to report, state "No actionable writing findings" and report verification.
