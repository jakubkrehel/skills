# Slow motion

How to slow an animation down and what to look for once it is slow.

## What shows up at a tenth of the speed

| Symptom | Usual cause |
| --- | --- |
| Text or an icon appears or vanishes at once while its container is still moving | The content is mounted and unmounted with no fade of its own |
| Content is clipped while the container grows | The content is added before the container has room |
| An empty gap sits in the container before it finishes shrinking | The content is removed before the container closes |
| The element jumps `1px`–`2px` on the first or last frame | The layer is promoted and dropped. See [performance.md](performance.md#use-will-change-sparingly) |
| A popup grows from its center while anchored to a trigger | No `transform-origin` |
| A shared element fades partway through its move | Its `layoutId` element carries `initial` or `exit` props |
| The height snaps on the first open and animates after | The first measurement was `0` |

Fade content over the same duration as the container it sits in, so neither finishes before the other.

## CSS transitions and animations

Chrome's Animations panel replays at `25%` and `10%` and lets you scrub. From a script, slow everything the browser itself is running:

```js
document.getAnimations().forEach((animation) => {
  animation.playbackRate = 0.1;
});
```

Run it right after triggering the state change, since it reaches only animations that exist at that moment.

## Values a library drives from JavaScript

`getAnimations()` misses these, and independent transforms such as `x` and `scale` on a `motion` element are among them. Multiply the transition's `duration` by `10` for the review and put it back afterwards:

```tsx
<MotionConfig transition={{ type: "spring", duration: 0.3 * 10, bounce: 0 }}>
```

`MotionConfig` reaches only children with no `transition` of their own, so multiply those in place.

A component that takes a `speed` prop and multiplies its own durations makes this a one-word change.

## A recording

Without a way to slow the page, record the screen and scrub the recording frame by frame. This is also how to study an animation somebody else built.
