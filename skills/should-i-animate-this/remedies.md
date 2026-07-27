# Remedies

Structural changes — which animation exists and when it fires. For exact values, easing and motion-library recipes, use `better-ui`.

`Make one-sided` has no single recipe; how you drop the enter depends on the component.

Match the project's styling system; the examples show plain CSS and the `motion` package.

## Cut

Delete it, then confirm the state change is still visible without it.

```css
/* Before: a full entrance replays on every accidental hover */
.row:hover .row-icon {
  animation: bounceIn 500ms;
}

/* After: instant, with no transition at all */
.row:hover { background-color: var(--surface-hover); }
```

Hover on anything the cursor sweeps across — list rows, table rows, menu items — is instant. A transition lags the pointer and leaves a trail of half-lit rows behind it.

If removing it leaves the user unable to tell what happened, the verdict isn't `Cut` — the design needs a static cue added first.

## First-run only

For expressive motion that's good the first time and friction after. Animate on the first visit; pass `initial={false}` on every later one so the element mounts in its final state with no entrance.

```tsx
<motion.div
  initial={firstRun ? { opacity: 0, y: 12 } : false}
  animate={{ opacity: 1, y: 0 }}
/>
```

`firstRun` comes from a persisted flag (e.g. `localStorage`, read on the client). Resolve it before the element mounts, or the entrance is gone by the time you learn it was the first run.

Usually the honest compromise when the ceiling condemns an animation the team is attached to.

## Shorten

Bring the duration under the ceiling and reduce to the cheapest properties that still communicate the change.

```css
/* Before: 400ms and four properties on a tab switch */
.tab-panel { transition: opacity 400ms, transform 400ms, filter 400ms, height 400ms; }

/* After: under the ceiling, one property, no layout animation */
.tab-panel { transition: opacity 120ms ease-out; }
```

Dropping the layout property matters as much as the duration: animating `height` shifts everything below it under the cursor.

## Keep

A justified animation, so the audit can recognize one. This modal scales from its trigger's position, answering *where did this come from*, and fires a few times a day.

```tsx
<motion.div
  initial={{ opacity: 0, scale: 0.96 }}
  animate={{ opacity: 1, scale: 1 }}
  exit={{ opacity: 0, scale: 0.96, transition: { duration: 0.15 } }}
  transition={{ duration: 0.2, ease: [0.2, 0, 0, 1] }}
  style={{ transformOrigin: triggerOrigin }}
/>
```

It answers a question the static frame can't, it's under the ceiling for its frequency, the content is legible before the transform settles, and the exit is shorter than the enter. Record the reason in **Considered but Kept**, not just the verdict.
