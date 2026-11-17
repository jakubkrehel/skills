# Reporting

The severity ladder and format for a standalone review. `better-interface` replaces both when it orchestrates.

**Severity.** `HIGH` makes text unreadable, truncates content with no way back to it or clips text at 320px width or 200% zoom. The last two are `better-interface` escalation triggers, so they stay `HIGH` however minor the surface. `MEDIUM` misses an exact value under **Measured, not preferred** in [SKILL.md](SKILL.md), or breaks the type system or the visual heading hierarchy. `LOW` is isolated polish.

**Format.** Group findings under the principle each violates, ordered by severity, one row per root cause listing every location it appears in:

| Severity | Location | Before | After | Why |
| --- | --- | --- | --- | --- |

`Location` is `path/to/file:line`. `Why` names the principle and the user impact.

End with `Block` when any `HIGH` remains, `Approve` otherwise, leaving the rest in the table as work to do. Never `Approve` coverage you did not inspect. With nothing to report, state "No actionable typography findings" and report verification.
