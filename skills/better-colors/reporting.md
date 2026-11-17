# Reporting

The severity ladder and format for a standalone review. `better-interface` replaces both when it orchestrates.

**Severity.** `HIGH` makes content unreadable or assigns a misleading semantic color. `MEDIUM` is a noticeable theme, token or gamut failure. `LOW` is isolated polish.

Three symptoms are `HIGH` on sight whatever the surface, matching `better-interface`'s escalation triggers. They are body or control text whose rendered pair fails its required ratio, state or meaning carried by color alone and a semantic color used against its meaning.

**Format.** Group findings under the principle each violates, ordered by severity, one row per root cause listing every location it appears in:

| Severity | Location | Before | After | Why |
| --- | --- | --- | --- | --- |

`Location` is `path/to/file:line`. `Why` names the principle and the user impact.

End with `Block` when any `HIGH` remains, `Approve` otherwise, leaving the rest in the table as work to do. Never `Approve` coverage you did not inspect. With nothing to report, state "No actionable color findings" and report verification.
