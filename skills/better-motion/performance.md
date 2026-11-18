# Performance

Recipes for transition specificity, compositing hints and loading Motion in parts.

## Transition only what changes

`transition: all` animates properties nobody meant to animate, such as colors on a theme flip. On layout properties such as `width` and `padding` it re-runs layout on every frame. Tailwind's bare `transition` is a curated list rather than `all`, and still covers more than most elements change.

```css
/* Good: only what changes */
.button {
  transition-property: scale, background-color;
  transition-duration: 150ms;
  transition-timing-function: ease-out;
}

/* Bad: everything */
.button {
  transition: all 150ms ease-out;
}
```

```tsx
// Good: explicit properties
<button className="transition-[scale,background-color] duration-150 ease-out">

// Bad: transition all
<button className="transition-all duration-150 ease-out">
```

In Tailwind 4, `transition-transform` covers `transform, translate, scale, rotate`, and in Tailwind 3 only `transform`. Use it when animating transforms alone. Mixing transform and other properties takes the bracket syntax, as in `transition-[scale,opacity,filter]`.

## Use will-change sparingly

`will-change` promotes an element to its own compositing layer ahead of time. Without it the browser promotes when the animation starts, and that one-time promotion can stutter on the first frame. Each extra layer costs memory, so never add it preemptively.

The same promotion can also move the element by `1px`–`2px` on the first or last frame, most visibly in Safari on iOS. `will-change: transform` keeps the layer for the whole animation and removes the jump.

```css
/* Good: names the property that animates */
.animated-card {
  will-change: scale, opacity;
}

/* Bad: never all */
.animated-card {
  will-change: all;
}

/* Bad: these never composite */
.animated-card {
  will-change: background-color, padding;
}
```

| Property | Worth `will-change` |
| --- | --- |
| `transform`, `translate`, `scale`, `rotate` | Yes |
| `opacity` | Yes |
| `filter` | Yes |
| `clip-path` | Rarely, since compositing it is not reliable across browsers |
| `top`, `left`, `width`, `height` | No |
| `background`, `border`, `color` | No |

`will-change` on a transform property or `filter` makes the element the containing block for its `position: fixed` descendants, so a fixed modal or tooltip inside it stops tracking the viewport. Any of these values also creates a stacking context, which can reorder `z-index` against siblings.

## Lazy Motion

`LazyMotion` loads Motion's features separately from the components, and the `m` components are the ones that work inside it:

```tsx
import { domMax, LazyMotion, MotionConfig } from "motion/react";
import * as m from "motion/react-m";

<LazyMotion features={domMax}>
  <MotionConfig reducedMotion="user">{children}</MotionConfig>
</LazyMotion>
```

| Feature set | Covers |
| --- | --- |
| `domAnimation` | Animations, variants, exits and the hover, tap and focus gestures |
| `domMax` | Everything in `domAnimation`, plus drag, pan and layout animation |

Follow what the project already does. Never add `LazyMotion` to a project that imports `motion` directly, since mixing the two loads the full bundle anyway.
