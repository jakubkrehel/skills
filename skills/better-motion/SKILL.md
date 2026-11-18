---
name: better-motion
description: Decides what should animate and builds it, with exact values for duration, easing, scale and distance and recipes for enters, exits, icon swaps, shared layout animation and drag.
---

# Motion

This skill decides whether an element should move and gives the values and recipes for when it should. It covers transitions, springs, enters and exits, icon swaps, shared layout animation, animated height and drag.

## Less motion, exact values

The first question is whether to animate at all, and the answer is often no. Add motion where the request asks for it or a principle here calls for it. Removing an animation is a valid fix.

The values come in two kinds. The press scale, the icon cross-fade, the stagger and the spring bounce are exact, so `0.96` is not `0.95`. The duration, scale and distance tables are starting points for desktop web. Adjust them by eye for the element's size and the distance it travels. Full-screen transitions on mobile run longer and travel farther.

Keep the project's motion library, tokens and curves, and match its motion language wherever no principle here gives a value. A deliberate and consistent project convention is a preference and not a finding. That covers a value too, so a project that presses to `0.95` on every button keeps it. The same detail done two ways within the project is a finding.

The reduced-motion requirement, pause controls and the single-pointer alternative to a drag belong to `better-accessibility`. Surfaces, icon weight and color and hover gating belong to `better-ui`. The layout an animation moves between belongs to `better-layout`.

## How often it is seen decides whether it moves

Keystrokes, row hovers and the content behind a tab change instantly, or with a transition of `150ms` or less on `opacity` or `background-color`. A tab's indicator may still travel between tabs. See [transitions.md](transitions.md#high-frequency-motion).

A menu people open many times a day skips its opening animation and animates only as it closes. A context menu and a command menu count, and a menu on a settings page does not. Reserve expressive motion for infrequent moments such as a view's first load, a success state or an empty state.

Every animated state change also leaves a static cue in the form of a color, icon or label. Motion is never the only feedback channel.

## Transitions, not keyframes, for interactive state

Drive interactive state changes with CSS transitions or a motion library's springs, which retarget when the user reverses mid-flight. Keyframes run a fixed timeline and cannot reverse, so reserve them for sequences that run once. See [transitions.md](transitions.md).

## Duration follows size

Small elements move fast and large ones take longer:

| Element | Enter |
| --- | --- |
| Tooltips and other small elements | `150ms` |
| Dropdowns and menus | `250ms` |
| Modals, drawers and panels | `350ms`–`450ms` |

An exit runs shorter than its enter, so a dropdown that opens in `250ms` closes in `150ms`.

## Ease out, and spring what has weight

Enters, exits and state changes take `ease-out`. Loading indicators and other continuous loops take `linear`. A sheet opening, a shape morphing, a drag settling and a slider thumb take a spring, because they imitate a physical object.

A color or opacity change never takes a spring, since nobody can see the difference. CSS has no practical spring. Without a motion library, stand in with `cubic-bezier(0.2, 0, 0, 1)` at the same duration.

## Springs take a duration and no bounce

Write a spring as `{ type: "spring", duration: 0.3, bounce: 0 }`, and bounce is always `0`. Use `0.3` for small swaps, `0.4` for a tab indicator and `0.45` for a drawer.

Where one surface shares a spring, set it once on `MotionConfig` and leave `transition` off the children. See [transitions.md](transitions.md#one-transition-for-the-surface).

## Scale shrinks as the element grows

A tooltip or dropdown enters from `0.96` or `0.97`. A modal or panel enters from `0.98` or `0.99`, because a large surface scaling from `0.95` changes too much of the screen. A modal or panel exits to `0.99`, a smaller change than its enter.

## Scale from the trigger

Set `transform-origin` toward the element that opened the popup, never the default center. Base UI exposes the side as `var(--transform-origin)`, and Radix as a variable such as `var(--radix-popover-content-transform-origin)`. A badge or indicator in a corner scales from that corner. A dialog centered on the screen keeps the center. See [enter-exit.md](enter-exit.md#origin-from-the-trigger).

## Movement shows where it came from

Travel only far enough to show direction:

| Movement | Distance |
| --- | --- |
| A text swap | `4px` |
| A page transition | `8px` |
| Dropdowns, tooltips and most elements | `8px`–`32px` |
| Sheets and side panels | Up to `70%` of their own width or height |

A dropdown moves away from its trigger, a panel enters from the side it was opened from and a tab indicator travels between the two tabs.

## Press scales to 0.96

A pressed button scales to `0.96` over `150ms` with `ease-out`, and a disabled one never scales. Give the button an opt-out where the motion would distract, through the component's existing variant API or a `static` prop. See [recipes for CSS, Tailwind and Motion](transitions.md#scale-on-press).

## Stagger infrequent entrances by 100ms

Where sequence communicates hierarchy in a staged entrance, split the content into semantic chunks such as title, description and actions. Stagger them `100ms` apart, each entering with opacity, `4px` of blur and `12px` of `translateY` over `300ms` with `ease-out`. Never stagger routine interactions. See [enter-exit.md](enter-exit.md).

## Exits are shorter and smaller than enters

Exit with opacity, `4px` of blur and a fixed `-12px` `translateY` over `150ms` with `ease-out`, never the full container height. Slide fully out only where the destination carries meaning, such as a drawer closing. Remove the element instantly where motion adds no information. See [enter-exit.md](enter-exit.md#exit-animations).

## Skip state animations on first render

Set `initial={false}` on an `AnimatePresence` that wraps a state swap, so the default state does not animate in on mount. Never set it around an intentional entrance such as a staggered hero. See [transitions.md](transitions.md#skip-animation-on-page-load).

## Cross-fade contextual icons with exact values

Where an icon swaps on an infrequent state change, such as play to pause or copy to copied, cross-fade it. Scale runs `0.25` to `1`, opacity `0` to `1` and blur `4px` to `0px`. With a motion library, use the `0.3` spring. Without one, keep both icons in the DOM and cross-fade over `300ms` with `cubic-bezier(0.2, 0, 0, 1)`.

A tab's icon swap and the actions a row reveals on hover fall under **How often it is seen decides whether it moves**. Both recipes are in [icon-transitions.md](icon-transitions.md).

## One layoutId per element on screen

Motion animates between two elements that share a `layoutId`, even when they render in different places with different sizes. The id is unique among the elements rendered at the same moment, and two at once break the animation.

Keep an element with a `layoutId` outside `AnimatePresence`, or its `initial` and `exit` run during the move. Pass a value that differs between the two states, such as `borderRadius`, through `animate` and never a class. See [layout-animation.md](layout-animation.md).

## Choose the AnimatePresence mode by where the children sit

| Mode | Use |
| --- | --- |
| `sync`, the default | The children occupy different places and can animate at once |
| `wait` | The new child may enter only after the old one has left |
| `popLayout` | The children cross-fade in the same place |

Every direct child of `AnimatePresence` carries a `key` that changes with the state, because the key change is what runs the exit and the enter. See [layout-animation.md](layout-animation.md#presence-modes).

## Measure the content to animate height

A container whose content changes stays at `height: auto` on both sides, so it snaps to the new size. Measure an inner wrapper with a `ResizeObserver` and animate the outer element to that number.

The measuring `ref` goes on the inner wrapper, never on the element whose height animates. The measurement is `0` until the content mounts, so give the first state a fixed height. Skip the animation where the content changes on every keystroke, such as a results list. See [layout-animation.md](layout-animation.md#animated-height).

## A drag ends where you decide

A row that snaps open and closed sets `dragMomentum={false}`, so it lands on a snap point and never where velocity throws it. Set `dragConstraints` to the outermost snap points and `dragElastic` to `0.05`. On release, animate to the nearest snap point with a spring. The commit threshold is half the distance to that point. See [drag.md](drag.md).

Prefer Motion's gesture props over hand-wired pointer events. `whileTap` also fires on the Enter key, and `whileHover` ignores touch. The table is in [drag.md](drag.md#gesture-props).

## Gate motion behind the reduced-motion preference

`better-accessibility` owns the requirement. Where a recipe in the reference files shows only the full-motion form, add this gate when you use it. A global reduced-motion reset in the project's stylesheet counts as the gate. Run movement, blur and entrance or exit scale only under `prefers-reduced-motion: no-preference`. The press scale stays, because it is brief feedback and not travel. Under reduced motion, replace them with an opacity cross-fade rather than removing the element instantly. Recipes for CSS, Tailwind and Motion are in [transitions.md](transitions.md#reduced-motion-fallback).

## Suppress transitions on theme switch

Disable every transition for the theme swap, force a style flush and restore transitions after the next frame. Otherwise every color transition fires at once and the switch smears. See the [recipe](transitions.md#suppress-transitions-on-theme-switch).

## Transition only what changes

Name the exact properties, as in `transition-property: scale, opacity`, and never `all`. Animate `transform`, `opacity`, `filter` and `clip-path`, which skip layout. Measured height is the one layout property worth animating. See [performance.md](performance.md).

## will-change fixes a jump you have seen

Add `will-change` only after you see the first frame stutter, or the element shift by `1px`–`2px` as its animation starts or settles. That shift shows most in Safari on iOS. Name the property you animate: `scale` for `scale`, `transform` for `transform`. Limit it to transform properties, `opacity` and `filter`, and never use `all`. See [performance.md](performance.md#use-will-change-sparingly).

## Load only the Motion features in use

Where the project wraps the app in `LazyMotion`, use the `m` components from `motion/react-m` and check which feature set it loads. `domAnimation` covers opacity, transforms, exits and the hover, tap and focus gestures. Shared layout animation and drag need `domMax`, and do nothing without it. See [performance.md](performance.md#lazy-motion).

## Slow it down before you call it done

Speed hides flaws. Replay every animation you built or changed at a tenth of its speed and step through it, then fix what shows and check again at full speed. The flaws to look for and the ways to slow each kind of animation are in [slow-motion.md](slow-motion.md).

## Before you finish

| Detection | Fix |
| --- | --- |
| `transition-all` or `transition: all` | Name the changing properties |
| `animation:` set on `:hover`, `.open` or another toggled class | A transition on the same properties |
| `active:scale-95`, `scale-90` or `whileTap` below `0.96`, in a project with no single press value | `0.96` |
| `:active` scale with no `:disabled` exclusion | `:not(:disabled):active`, or `enabled:active:` on an element that is never a link |
| `bounce:` set to anything but `0` | `bounce: 0` |
| A spring on a transition that changes only color or opacity | `ease-out` with a duration |
| A modal or panel entering from a `scale` below `0.98` | `0.98` |
| A popup anchored to a trigger with no `transform-origin`, or `origin-center` | The library's origin variable |
| An exit with the same duration and distance as its enter | `150ms` and a fixed `12px` |
| `opacity: 0` in a base rule that only an `animation` reveals | Hidden state in the `from` keyframe, `animation-fill-mode: both` |
| `translate`, `filter`, `motion.*`, `m.*` or a `scale` other than the press with no `prefers-reduced-motion`, `motion-safe:`, `MotionConfig` or global guard | Gate it and cross-fade opacity under reduced motion |
| `filter` animated by Motion with `MotionConfig reducedMotion` as its only guard | Branch on `useReducedMotion()` |
| Color transitions plus a theme toggle with no `disableTransitionOnChange` or override | The theme-switch suppression recipe |
| `will-change: all`, or `will-change` on an element that never animates | Name the animated property or delete it |
| One `layoutId` on two elements rendered together | A unique id for each |
| A `layoutId` element directly inside `AnimatePresence` with `initial` or `exit` props | Move it outside |
| A direct child of `AnimatePresence` with no `key` | Key it by the state |
| `height: "auto"` animated on a container whose children swap | The measured height |
| A measuring `ref` on the element whose height animates | An inner wrapper |
| A snapping drag with `dragMomentum` left on | `dragMomentum={false}` |
| `layoutId` or `drag` under `LazyMotion features={domAnimation}` | `domMax` |

## Reporting

**Verification.** Without a browser, read every animated state from the code with its properties, durations, easings and origin. With one, walk each state, reverse it mid-flight and replay it slowed as [slow-motion.md](slow-motion.md) describes. Report every check you could not run as `Not verified`.

A standalone review takes its severity ladder and format from [reporting.md](reporting.md).
