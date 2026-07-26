# Frequency and duration

How often an interaction fires decides how much time its animation may spend.

## Ceilings by frequency

Classify the trigger, then apply the ceiling. Classify by what the interaction *is* — right-click on a canvas app is constant, account settings is rare — not by how often you triggered it while building.

| How often the user triggers it | Ceiling | What's allowed |
| --- | --- | --- |
| Constantly — hover, row select, keystroke, tab switch, toggle | none, or ≤150ms | `opacity` and `background-color` only |
| Many times a day — menus, dropdowns, popovers | ≤200ms, or make it one-sided | transform and opacity |
| A few times a session — modals, drawers, navigation | ≤300ms enter, ≤200ms exit | transform and opacity |
| Once a session or once ever — first load, onboarding, celebration | ≤400ms | anything it earns |

Hover on anything the cursor sweeps across — list rows, table rows, menu items — is instant, with no transition. A transition lags the pointer and leaves a trail of half-lit rows behind it. The ≤150ms allowance is for discrete controls the user aims at and clicks, like a toggle or a button.

**400ms is the hard ceiling regardless of frequency.** Under 100ms reads as instant, 200–300ms is the working range, past 400ms the user perceives waiting rather than transition, past 1s they assume something is loading. Only distance justifies going over: a full-screen sheet travelling the height of the viewport, never a dropdown moving 8px.

Exits should be shorter than their ceiling and may never be longer than their enter.

**Rule:** The ceiling is a maximum, not a target. Cut the duration before cutting the clarity.

## Enter and exit are not symmetric

A right-click menu in a canvas app, opening and closing at 300ms.

Entering, the user is waiting to read the items, so every millisecond is latency. Leaving, the item is chosen and attention has moved to the result, so the motion is free.

Verdict: `Make one-sided`. Instant open, 150ms close.

## Count what the code tells you

Never guess a number you can't observe. Triggers per day is unknowable from source, so classify it into a bucket and say which. Two totals *are* readable from the code, and both are worth reporting.

**Flow total.** A six-step checkout, each step transitioning at 200ms:

```
6 steps × 200ms = 1.2 seconds added to every checkout
```

Every animation passes on its own; the finding is the total. Cut the transitions on steps that are pure progression, keep the ones carrying spatial meaning.

**Stagger total.** A 50-row list, each row entering 40ms after the previous:

```
last row lands at 50 × 40ms = 2.0 seconds
```

The honest duration of a stagger is when the **last** item arrives. Verdict: `Cut`. Stagger is for a handful of semantic chunks in a hero, not for list rows.

## Free vs. manufactured latency

Motion overlapping time the user was already spending costs nothing: a transition running during a network request, a skeleton in a genuine loading window, an exit playing after intent is fulfilled.

Motion on an instant operation manufactures a wait: a 300ms transition on a local state toggle, an entrance on content already in memory, a flourish on a synchronous calculation.

```css
/* Good: covers a real round-trip already in progress */
.results { transition: opacity 200ms ease-out; }

/* Bad: a local filter is instant, so 300ms of fade is invented latency */
.results.filtered { animation: fadeInUp 300ms; }
```

**Rule:** Ask what the interface would be doing during those milliseconds without the animation. If the answer is "nothing, it would be done", the animation is the wait.

## Interruptibility is the escape hatch

An animation that retargets on re-trigger lets the user who doesn't want to wait skip it. One that must play out charges its full duration every time.

**Rule:** Motion on a frequent interaction must be interruptible or it doesn't ship. This can rescue a borderline animation the ceiling would otherwise cut — never one on the critical path, where the user still can't read until it settles.

`better-ui` owns the implementation.

## Reduced motion is not the remedy

An animation too expensive for its frequency is too expensive for everyone. Gating it behind `prefers-reduced-motion` fixes it for the fraction who set the preference and leaves the cost for everyone else.

`better-accessibility` owns the `prefers-reduced-motion` requirement, which applies to every animation that survives this audit.
