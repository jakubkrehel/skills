# Reporting

The severity ladder and format for a standalone review. `better-interface` replaces both when it orchestrates.

**Severity.** `HIGH` blocks content or an action at a supported viewport. Clipping or overlap at 320px width or 200% zoom is always `HIGH`, as is truncated content with no way to the full value. So is content past a scroll edge or behind a disclosure with no visible cue. `MEDIUM` harms hierarchy, reading order or adaptability. `LOW` is isolated alignment or spacing polish.

**Format.** Group findings under the principle each violates, ordered by severity, one row per root cause listing every location it appears in:

| Severity | Location | Before | After | Why |
| --- | --- | --- | --- | --- |

`Location` is `path/to/file:line`. `Why` names the principle and the user impact.

End with `Block` when any `HIGH` remains, `Approve` otherwise, leaving the rest in the table as work to do. Never `Approve` coverage you did not inspect. With nothing to report, state "No actionable layout findings" and report verification.
