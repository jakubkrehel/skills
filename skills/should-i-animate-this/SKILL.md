---
name: should-i-animate-this
description: Decide whether an animation belongs at all, and how much time it may spend, based on how often the user triggers it. Use when an interface feels slow or busy, when reviewing existing motion, or before adding an animation to something people use often. Triggers on should I animate this, is this animation necessary, is this animation worth it, motion audit, animation audit, motion restraint, animation budget, too many animations, remove animation, cut animation, animation feels slow, interface feels sluggish, high-frequency interaction, animation duration, enter and exit animation, is the animation in the way.
---

# Should this be animated?

Motion costs the user time, charged on every trigger. That cost is invisible while building it: you trigger the interaction five times, they trigger it two hundred times a day.

So the default is no. An animation earns its place by explaining something the end state can't, and the more often it fires, the less it gets to spend.

`better-ui` owns how an animation is built once it belongs. `better-accessibility` owns `prefers-reduced-motion`.

## Quick Reference

| Category | When to Use |
| --- | --- |
| [Frequency and duration](frequency-and-duration.md) | Ceilings by trigger frequency, path and stagger totals, free vs. manufactured latency |
| [Remedies](remedies.md) | Cut, shorten, first-run-only and keep, in code |

## Core Principles

### 1. Default to No Motion

The burden is on the animation to justify itself, not on the reviewer to justify removing it. If nobody can say what it explains, cut it.

### 2. The More Often It Fires, the Less It Spends

Anything the cursor sweeps across — list rows, menu items — changes instantly. Discrete controls the user aims at get ≤150ms of `opacity` or `background-color`. Menus and dropdowns get ≤200ms, modals and navigation ≤300ms in and ≤200ms out, once-ever moments can spend. Full table in [frequency-and-duration.md](frequency-and-duration.md).

### 3. Nothing Over 400ms Without a Reason

Independent of frequency: under 100ms reads as instant, 200–300ms is the working range, past 400ms the user perceives waiting rather than transition. The only reason that justifies going over is distance — a full-screen surface, never a dropdown.

### 4. Never Put Motion on the Critical Path

If the user can't read the content or hit the control until it finishes, you invented latency. A 200ms delay before a menu is usable is worse than a 400ms flourish nobody waits on.

### 5. Audit the Flow, Not the Component

Six steps at 200ms is 1.2 seconds added to a checkout, and every one passes on its own. Report the path total as its own finding.

### 6. Motion Must Answer a Question the End State Can't

Origin, destination, continuity of identity, causality, duration. If it answers none of those it's decoration, which is allowed only where the frequency budget is generous.

### 7. Delete It First

Remove the animation and use the interface. Not missing it is the answer; if the interface becomes confusing, the design is leaning on motion and needs a static cue, not a better transition.

### 8. One Thing Moves at a Time

Motion only emphasizes when it's scarce. When several elements animate on one trigger, coordinate them as a single movement or cut all but the one that matters.

### 9. Loops Are a Permanent Tax

Shimmer, pulsing dots, breathing gradients and rotating marks charge attention for as long as they're on screen. Reserve them for genuinely ongoing states: loading, recording, live data.

### 10. Budget Scales Inversely With Dwell Time

A tool someone lives in all day approaches zero expressive motion, because speed is the feature there. A page seen once can spend.

### 11. Never Animate the Primary Content on Load

The user came for the content, and fading or sliding it in makes it unreadable for the duration. Animate the chrome instead. This applies with full force to scroll-triggered reveals in feeds, tables and lists.

### 12. Never Animate an Interactive Target Into Position

People aim ahead of the cursor, so a control that slides or reflows into place gets mis-clicked. Fade it in place, and don't animate height where something clickable sits below.

### 13. Direct Manipulation Follows Input 1:1

While the user is dragging or swiping, the element tracks the input with no duration, easing or delay. Motion may resume when the gesture ends.

## Common Mistakes

| Mistake | Fix |
| --- | --- |
| Entrance animation on every hover or keystroke | Instant, or ≤150ms opacity/color |
| Judging frequency from building or demoing it | Classify by what the interaction is, not how often you triggered it |
| Each animation in a flow reviewed alone | Add up the whole path |
| Per-item stagger on a list that can be long | Animate the container, or nothing |
| Animation on an instant local operation | Cut it, or overlap it with work that takes real time |
| Decorative loop in persistent chrome | Reserve loops for ongoing states |
| Content faded or blurred in on page load | Animate the chrome; leave the content readable |
| Motion is the only signal that state changed | Add a static cue: color, icon or label |
| Tuning the duration of an animation that shouldn't exist | Delete it first |

## Review Output Format

Use this format when asked to audit motion. If another skill is orchestrating a broader review, supply these findings as evidence and let that skill's format take precedence.

### Findings

One row per animation, columns **Verdict**, **Severity**, **Location**, **Frequency**, **Change**, **Why**.

- **Verdict**: exactly one of `Cut`, `Make one-sided`, `First-run only`, `Shorten`, `Keep`.
- **Severity**: `HIGH` is frequent and on the critical path; `MEDIUM` is noticeable but infrequent or off it; `LOW` is isolated.
- **Location**: `path/to/file:line`, or the exact screen and component when there are no source files.
- **Frequency**: the bucket from [frequency-and-duration.md](frequency-and-duration.md) and the interaction it describes. Never invent a triggers-per-day number.
- **Change**: the concrete edit. See [remedies.md](remedies.md).
- **Why**: the violated principle and what the user loses.

Consolidate systemic issues into one row listing every location. When auditing a flow, add a total row for the path.

### Example

| Verdict | Severity | Location | Frequency | Change | Why |
| --- | --- | --- | --- | --- | --- |
| Make one-sided | HIGH | `src/ContextMenu.tsx:34` | Many times a day — right-click is the primary action | Remove the enter; keep the 150ms exit | The user is waiting to read the options, so entering is pure latency |
| Cut | MEDIUM | `src/Row.tsx:12` | Constantly — every row hover | Delete `bounceIn`; make the `background-color` change instant | A 500ms entrance replays on accidental hovers, and any transition lags the cursor |
| Keep | LOW | `src/Modal.tsx:18` | A few times a session | No change | Animates from the trigger, so it answers where the modal came from |

### Considered but Kept

List 2–5 animations inspected and left alone, with the reason. An audit biased toward removal must show where it declined to remove.

### Verification and Verdict

1. **Verification**: name the frequency bucket assigned to each animation and what it was based on. Note any animation not inspected.
2. **Verdict**: `Block` if any `HIGH` remains, `Needs changes` if only `MEDIUM` or `LOW` remain, `Approve` when none do.

When every animation is justified, omit the findings table, state "No actionable motion findings", keep Considered but Kept, and end with `Approve`.
