# Reporting

The severity ladder and format for a standalone review. `better-interface` replaces both when it orchestrates.

**Severity.** `HIGH` breaks an interaction, as a keyframe toggle that cannot reverse or a hover state stuck on touch does. Two of `better-interface`'s escalation triggers land here and are `HIGH` on sight. One is motion that ignores `prefers-reduced-motion`, and the other is a state change carried by motion alone. `MEDIUM` is a behavior defect short of a broken interaction, such as scroll chaining, or a visible inconsistency in surfaces, icons or motion. `LOW` is isolated polish.

**Format.** Group findings under the principle each violates, ordered by severity, one row per root cause listing every location it appears in:

| Severity | Location | Before | After | Why |
| --- | --- | --- | --- | --- |

`Location` is `path/to/file:line`. `Why` names the principle and the user impact.

End with `Block` when any `HIGH` remains, `Approve` otherwise, leaving the rest in the table as work to do. Never `Approve` coverage you did not inspect. With nothing to report, state "No actionable UI-polish findings" and report verification.
