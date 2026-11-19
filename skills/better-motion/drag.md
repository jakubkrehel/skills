# Drag and gestures

Recipes for a drag that snaps, actions revealed by distance and Motion's gesture props. The single-pointer alternative every drag needs belongs to `better-accessibility`.

## A row that snaps

The outer container clips with `overflow: clip`, which hides the overflow without making it scrollable. The row inside carries the drag and one motion value for its offset:

```tsx
const SNAP = { left: -116, center: 0, right: 116 };
const THRESHOLD = 58;
const SPRING = { type: "spring", duration: 0.3, bounce: 0 } as const;

function Row({ children }) {
  const x = useMotionValue(0);

  const handleDragEnd = () => {
    const current = x.get();
    if (Math.abs(current) > THRESHOLD) {
      animate(x, current > 0 ? SNAP.right : SNAP.left, SPRING);
    } else {
      animate(x, SNAP.center, SPRING);
    }
  };

  return (
    <div className="overflow-clip">
      <motion.div
        drag="x"
        dragConstraints={{ left: SNAP.left, right: SNAP.right }}
        dragElastic={0.05}
        dragMomentum={false}
        onDragEnd={handleDragEnd}
        style={{ x }}
      >
        {children}
      </motion.div>
    </div>
  );
}
```

| Prop | What it sets |
| --- | --- |
| `drag="x"` | Locks the drag to one axis |
| `dragConstraints` | The farthest the row travels, here the outer snap points |
| `dragElastic` | How far the row stretches past a constraint, where `0.05` allows almost none |
| `dragMomentum={false}` | Removes the fling, so the release position alone decides the snap |

Keep the snap points and the threshold as named pixel constants. They are the values that get tuned by eye.

## Actions revealed by distance

Reveal each action as the row passes its own threshold, never all at once. Store how many actions are showing and never the raw offset, so the row re-renders only as it crosses a threshold:

```tsx
const [revealed, setRevealed] = useState(0);
useMotionValueEvent(x, "change", (latest) => {
  setRevealed(latest > 88 ? 2 : latest > 44 ? 1 : 0);
});

<ActionButton isVisible={revealed >= 1} label="Set reminder" />
<ActionButton isVisible={revealed >= 2} label="Mark as unread" />
```

```tsx
<motion.button
  animate={{ opacity: isVisible ? 1 : 0, scale: isVisible ? 1 : 0.5 }}
  aria-label={label}
  inert={!isVisible}
  initial={{ opacity: 0, scale: 0.5 }}
/>
```

An action at `opacity: 0` still takes focus and clicks, so `inert` removes it from the tab order and the hit area until it shows. React before 19 drops a boolean `inert`, so use `disabled` there.

A status dot that appears after the action completes scales from its own corner:

```tsx
<AnimatePresence>
  {isUnread && (
    <motion.div
      animate={{ opacity: 1, scale: 1 }}
      className="origin-bottom-right"
      exit={{ opacity: 0, scale: 0.75 }}
      initial={{ opacity: 0, scale: 0.75 }}
      key="unread"
      transition={{ type: "spring", duration: 0.3, bounce: 0 }}
    />
  )}
</AnimatePresence>
```

## Gesture props

| Prop | Fires | Worth knowing |
| --- | --- | --- |
| `whileHover` | While a pointer is over the element | Ignores touch, so it never latches after a tap |
| `whileTap` | From press to release on the same element | Fires on the Enter key too |
| `whileDrag` | While the element is dragged | Needs the `drag` prop |
| `whileFocus` | While the element has focus | Follows the `:focus-visible` rules |
| `whileInView` | While the element is in the viewport | `viewport={{ amount: 0.5 }}` sets how much must show |
| `onPan` | After the pointer moves `3px` while pressed | Has no `while` prop, so set state in `onPanStart` and `onPanEnd` |

Keep a `whileFocus` animation small. It supports the focus indicator `better-accessibility` requires and never replaces it.

A slider thumb, or anything else that follows the pointer and then settles, takes a spring.
