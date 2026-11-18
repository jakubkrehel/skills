# Enter and exit animations

Recipes for staged entrances and the exits that follow them. For interactive state feedback see [transitions.md](transitions.md), and for icon swaps see [icon-transitions.md](icon-transitions.md).

## Enter animations

1. **Split** into semantic groups such as title, description and actions.
2. **Stagger** the groups `100ms` apart.
3. **Split a short display headline** into words, staggered `80ms` apart, where it should read word by word.
4. **Combine** `opacity`, `blur` and `translateY` for each group's enter.

### Motion

```tsx
const item = {
  hidden: { opacity: 0, y: 12, filter: "blur(4px)" },
  visible: {
    opacity: 1,
    y: 0,
    filter: "blur(0px)",
    transition: { duration: 0.3, ease: "easeOut" },
  },
};

function PageHeader() {
  return (
    <motion.div
      initial="hidden"
      animate="visible"
      variants={{ visible: { transition: { staggerChildren: 0.1 } } }}
    >
      <motion.h1 variants={item}>Welcome</motion.h1>
      <motion.p variants={item}>A description of the page.</motion.p>
      <motion.div variants={item}>
        <Button>Get started</Button>
      </motion.div>
    </motion.div>
  );
}
```

Recent Motion versions deprecate `staggerChildren` in favor of `delayChildren: stagger(0.1)`. Use the one the installed version documents.

### CSS

The hidden state lives in the `from` keyframe, so an item whose animation never runs stays visible. Reduced motion keeps only the opacity keyframe.

```css
.stagger-item {
  animation: fade-in 300ms ease-out both;
}

@media (prefers-reduced-motion: no-preference) {
  .stagger-item {
    animation-name: fade-in-up;
  }
}

.stagger-item:nth-child(2) { animation-delay: 100ms; }
.stagger-item:nth-child(3) { animation-delay: 200ms; }

@keyframes fade-in {
  from { opacity: 0; }
}

@keyframes fade-in-up {
  from {
    opacity: 0;
    transform: translateY(12px);
    filter: blur(4px);
  }
}
```

### Entering from display: none

A popover, dialog or element toggled from `display: none` has no previous style to transition from. `@starting-style` supplies one, and `transition-behavior: allow-discrete` keeps it rendered through the exit. The open state's duration applies on enter and the closed state's on exit.

```css
[popover] {
  opacity: 0;
  transition-property: opacity, display, overlay;
  transition-duration: 150ms;
  transition-timing-function: ease-out;
  transition-behavior: allow-discrete;
}

[popover]:popover-open {
  opacity: 1;
  transition-duration: 250ms;
}

@starting-style {
  [popover]:popover-open {
    opacity: 0;
  }
}

@media (prefers-reduced-motion: no-preference) {
  [popover] {
    translate: 0 12px;
    transition-property: opacity, translate, display, overlay;
  }

  [popover]:popover-open {
    translate: 0 0;
  }

  @starting-style {
    [popover]:popover-open {
      translate: 0 12px;
    }
  }
}
```

`@starting-style` and discrete `display` transitions are Baseline 2024. `overlay` is Chromium only, and browsers without support show and hide instantly.

## Origin from the trigger

The popup scales from the side its trigger sits on. Positioning libraries publish that side as a variable, so the origin follows when the popup flips:

```css
/* Base UI */
.popup {
  transform-origin: var(--transform-origin);
}

/* Radix */
.popover-content {
  transform-origin: var(--radix-popover-content-transform-origin);
}
```

Radix names the variable after the component, as in `--radix-dropdown-menu-content-transform-origin` and `--radix-tooltip-content-transform-origin`. Without a library, set it by hand, such as `transform-origin: top left` for a menu that opens below the left edge of its button.

## Menus opened all day

A context menu or a command menu opens at once and animates only as it closes. With Base UI, style the ending state and leave the starting state alone:

```css
.popup {
  transition: opacity 150ms ease-out, scale 150ms ease-out;
}

.popup[data-ending-style] {
  opacity: 0;
  scale: 0.99;
}
```

## Exit animations

### Subtle exit

```tsx
<motion.div
  exit={{
    opacity: 0,
    y: -12,
    filter: "blur(4px)",
    transition: { duration: 0.15, ease: "easeOut" },
  }}
>
  {content}
</motion.div>
```

### Full exit

Slide fully out only where the destination carries meaning, such as a card returning to a list or a drawer closing:

```tsx
<motion.div
  exit={{
    opacity: 0,
    x: "-100%",
    transition: { duration: 0.2, ease: "easeOut" },
  }}
>
  {content}
</motion.div>
```

### Good and bad

```css
/* Good: subtle exit */
.item-exit {
  opacity: 0;
  transform: translateY(-12px);
  transition: opacity 150ms ease-out, transform 150ms ease-out;
}

/* Bad: dramatic exit that steals focus */
.item-exit {
  opacity: 0;
  transform: translateY(-100%) scale(0.5);
  transition: all 400ms ease-out;
}

/* Sometimes correct: remove at once when motion adds no information */
.item-exit {
  display: none;
}
```
