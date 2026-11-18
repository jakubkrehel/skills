# Layout animation

Recipes for shared layout animation, `AnimatePresence` modes and animated height with Motion. Enter and exit values live in [enter-exit.md](enter-exit.md).

## Shared layout animation

Each state renders its own element, and the elements share a `layoutId`. Motion measures both and animates from one to the other, so nothing is moved by hand.

```tsx
function Card({ expanded }) {
  return expanded ? (
    <motion.div className="absolute inset-4" layoutId="card" />
  ) : (
    <motion.div className="size-16" layoutId="card" />
  );
}
```

Several elements can move at once when each pair has its own id, as in `circle-1`, `circle-2` and `circle-3`. Unrelated elements that share an id animate into each other, so build ids in a list from the item's key.

### Values that differ between states

Position and size come from each state's classes. A value Motion has to interpolate through the move, such as the radius, goes through `animate`:

```tsx
<motion.div
  animate={{ borderRadius: expanded ? "8px" : "32px" }}
  className={cn("absolute bg-surface", expanded ? "inset-4" : "size-8")}
  layoutId="card"
/>
```

### With AnimatePresence

Content that fades while the shared element moves sits inside `AnimatePresence`. A shared element that swaps one state for the other sits outside it:

```tsx
<AnimatedCircle layoutId="circle-1" />

<AnimatePresence>
  {showArrows && (
    <motion.div
      animate={{ opacity: 1, filter: "blur(0px)" }}
      exit={{ opacity: 0, filter: "blur(4px)" }}
      initial={{ opacity: 0, filter: "blur(4px)" }}
      key="arrows"
    />
  )}
</AnimatePresence>

<AnimatedCircle layoutId="circle-2" />
```

A `layoutId` element runs its `initial` and `exit` during the move, and an opacity exit makes it fade mid-flight. Give it neither.

### Tab indicator

Render the indicator only inside the active tab and let the shared id carry it across. The label sits above it:

```tsx
{tabs.map((tab) => (
  <button
    aria-pressed={active === tab}
    className="relative"
    key={tab}
    onClick={() => setActive(tab)}
    type="button"
  >
    {active === tab && (
      <motion.div
        className="absolute inset-0 rounded-xl bg-surface-active"
        layoutId="tab-indicator"
        transition={{ type: "spring", duration: 0.4, bounce: 0 }}
      />
    )}
    <span className="relative z-10">{tab}</span>
  </button>
))}
```

Two tab groups on one page need two different ids.

### Card to dialog

The card and the dialog popup are separate components that share a `layoutId`, and the card stays mounted behind the dialog. Motion cross-fades two mounted elements that share an id. `AnimatePresence` keeps the popup in the DOM on close until it has animated back to the card:

```tsx
<motion.button layoutId={`card-${id}`} onClick={() => setOpen(true)} />

<AnimatePresence>
  {open && <motion.div layoutId={`card-${id}`} />}
</AnimatePresence>
```

The popup takes no `initial` or `exit` props. Keep the project's dialog primitive for focus and dismissal, and render the popup as a `motion` element.

## Presence modes

| Mode | Behavior |
| --- | --- |
| `sync` | Enter and exit start together, and both children take up space |
| `wait` | The entering child waits until the exiting one has finished |
| `popLayout` | The exiting child leaves the layout at once, so the entering one takes its place |

A cross-fade between steps of one dialog uses `popLayout`, opacity and a small scale:

```tsx
<AnimatePresence initial={false} mode="popLayout">
  <motion.div
    animate={{ opacity: 1, scale: 1 }}
    exit={{ opacity: 0, scale: 0.95 }}
    initial={{ opacity: 0, scale: 0.95 }}
    key={step}
  >
    <StepContent step={step} />
  </motion.div>
</AnimatePresence>
```

`popLayout` positions the exiting child absolutely, so the parent needs a `position` other than `static`. A custom component used as the direct child has to pass its `ref` to the DOM node.

## Animated height

The outer element animates to a measured number and the inner wrapper is what gets measured:

```tsx
const [ref, bounds] = useMeasure({ offsetSize: true });

<motion.div animate={{ height: step === "default" ? 388 : bounds.height }}>
  <div ref={ref}>{content}</div>
</motion.div>
```

`bounds.height` is `0` until the content mounts and is measured. A dialog that opens on a measured height therefore grows from nothing, which is why the first state above takes a fixed number.

Where the project has no measuring hook, this one is enough:

```tsx
import { useEffect, useState } from "react";

export function useMeasure<T extends HTMLElement>({ offsetSize = false } = {}) {
  const [node, setNode] = useState<T | null>(null);
  const [bounds, setBounds] = useState({ width: 0, height: 0 });

  useEffect(() => {
    if (!node) return;

    const update = () => {
      if (offsetSize) {
        setBounds({ width: node.offsetWidth, height: node.offsetHeight });
        return;
      }
      const { width, height } = node.getBoundingClientRect();
      setBounds({ width, height });
    };

    update();
    const observer = new ResizeObserver(update);
    observer.observe(node);
    return () => observer.disconnect();
  }, [node, offsetSize]);

  return [setNode, bounds] as const;
}
```

`offsetSize` reads the layout size. `getBoundingClientRect` reads the painted size, which is wrong while an ancestor is scaling in.
