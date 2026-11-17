# Reporting

The severity ladder and format for a standalone review. `better-interface` replaces both when it orchestrates.

**Severity.** `HIGH` prevents a task, hides content from assistive technology or creates a systemic failure. `MEDIUM` makes an interaction meaningfully harder. `LOW` is isolated polish. This domain's share of `better-interface`'s escalation triggers is `HIGH` on sight. That is a missing accessible name, a missing focus indicator and a pointer path with no keyboard path. It is also motion ignoring reduced motion, loss at 320px or 200% and meaning carried by color alone.

**Format.** Group findings under the principle each violates, ordered by severity, one row per root cause listing every location it appears in:

| Severity | Location | Before | After | Why |
| --- | --- | --- | --- | --- |

`Location` is `path/to/file:line`. `Why` names the WCAG criterion or principle and the user impact.

End with `Block` when any `HIGH` remains, `Approve` otherwise, leaving the rest in the table as work to do. Never `Approve` coverage you did not inspect. With nothing to report, state "No actionable accessibility findings" and report verification.
