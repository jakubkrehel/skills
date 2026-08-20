# Review Output Format

Use one consolidated report rather than a list of disconnected polish suggestions.

## Scope and coverage

State the exact screen, flow, feature, or change reviewed; the stack and styling conventions; the viewports and states inspected; and any project guidance documents read.

| Area | Evidence inspected | Result |
| --- | --- | --- |
| Product specificity | Screens, copy, assets, design contract | Findings or `Clear` |
| Interaction states | Primary, loading, empty, error, disabled, success | Findings or `Clear` |
| Responsive behavior | Wide and narrow viewports, long content | Findings or `Clear` |
| Accessibility | Keyboard path, focus, labels, contrast, motion | Findings or `Clear` |
| Technical finish | Console, links, assets, dead controls | Findings or `Clear` |

## Findings

| # | Severity | Location | Before | After | Why |
| --- | --- | --- | --- | --- | --- |
| 1 | HIGH / MEDIUM / LOW | `path/to/file:line` or screen and component | Current behavior | Concrete replacement | User impact and violated principle |

Consolidate repeated symptoms into one root-cause finding. Use `HIGH` for blocked tasks, inaccessible controls, misleading states, or clipped/unreachable content; `MEDIUM` for meaningful comprehension or efficiency problems; and `LOW` for isolated polish.

## Considered but rejected

List one to three real candidates that were inspected and left unchanged, with the evidence or project convention that justified the decision.

## Verification

List each command or interaction and its observed result. Mark anything not run as `Not verified`; do not imply that a visual or runtime check happened when it did not.

## Verdict

End with exactly one: `Block`, `Needs changes`, or `Approve`.
