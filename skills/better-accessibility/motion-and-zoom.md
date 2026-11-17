# Motion and zoom

`prefers-reduced-motion`, timed UI, zoom and reflow.

## prefers-reduced-motion

Wrap animations in `@media (prefers-reduced-motion: no-preference)`, so users who asked for reduced motion get the static version without an override per animation.

```css
/* Good: motion is opt-in */
.card {
  /* static styles */
}
@media (prefers-reduced-motion: no-preference) {
  .card {
    transition: transform 200ms ease-out;
  }
}
```

```tsx
// Tailwind: motion-safe / motion-reduce variants
<div className="motion-safe:transition-transform motion-safe:hover:-translate-y-1" />
```

### What to disable vs reduce

Reduced motion targets vestibular triggers, not feedback.

| Disable entirely | Replace | Keep |
| --- | --- | --- |
| Parallax scrolling | Slide/scale/zoom transitions → opacity cross-fade | Loading spinners and progress |
| Autoplaying video, GIFs, looping decoration | Smooth scrolling → instant jump | Instant state changes (hover color, focus ring) |
| Spinning, large-scale movement across the screen | Auto-rotating carousels → start paused | Brief functional feedback (button press) |

## Autoplay and timed UI

- **Looping media counts** (2.2.2). A muted looping hero video needs the pause, stop or hide control too.
- **Explicit dismissal over timers.** Auto-dismissal suits low-stakes confirmations and nothing else.
- **Pause on attention.** Where a toast must time out, give it at least 5 seconds, and pause the timer while it is hovered or focused.
- **Never put critical information only in a timed element.** A vanished toast with the only link to an undo action is data loss on a schedule.

## Zoom and reflow

The 320px of 1.4.10 is a 1280px viewport at 400% zoom. Genuinely 2D content is the exception: tables, maps and code blocks scroll inside their own container.

Write media queries in `rem` or `em` where the codebase allows. At a larger browser font size such a query switches to the narrow layout when the text needs it, and a `px` query does not. Breakpoint placement belongs to `better-layout`.
