---
name: better-ui
description: Polishes surfaces, icons and motion with exact values for border radius, optical alignment, shadows, icon states and animation.
---

# UI polish

This skill holds the visual polish for surfaces, icons and motion, with the exact value each detail takes. It applies once the underlying interaction is sound, and a polish finding never outranks a broken interaction.

## Exact values, optional polish

The values below are exact, not ranges to approximate. `cubic-bezier(0.2, 0, 0, 1)` is not `cubic-bezier(0.4, 0, 0.2, 1)`, and `0.96` is not `0.95`. The optical nudges and the concentric padding cutoff are the exceptions. They are starting points, judged by eye.

Keep the project's component library, tokens and density, and match its motion language wherever no rule here gives a value. A deliberate and consistent project convention, such as a style with no shadows, is a preference and not a finding. That covers a value too, so a project that presses to `0.95` on every button keeps it. The same detail done two ways within the project is a finding.

Text wrapping, font rendering, tabular numbers and text spacing belong to `better-typography`. Hit areas, keyboard support, ARIA and the reduced-motion requirement belong to `better-accessibility`. Grouping, section spacing, breakpoints and spatial RTL belong to `better-layout`, except directional icon mirroring. Color tokens and contrast measurement belong to `better-colors`.

## Outer radius equals inner radius plus padding

Where nested surfaces share a visible, even inset, the outer radius is the inner radius plus the padding plus any border width. Past `24px` of padding, or where the padding is deliberately asymmetric, treat the layers as separate surfaces and keep each one's radius token. Recipes are in [surfaces.md](surfaces.md).

## Align optically where geometry looks off

Where geometric centering looks off, nudge by eye. Give a button `2px` less padding on its icon side, shift a play triangle toward its point and fix asymmetric glyphs in the SVG itself. Recipes are in [surfaces.md](surfaces.md#optical-alignment).

## Shadows for elevation, borders for structure

Where a border exists only to create depth, replace it with layered transparent `box-shadow` values. Keep borders on dividers, separators, table cells and selected states. Keep them on form inputs too, whose boundary needs 3:1 non-text contrast under `better-accessibility`. Focus rings belong to `better-accessibility` as well.

Forced-colors mode removes every `box-shadow`. Keep `border: 1px solid transparent` under a shadow ring so that mode still draws an edge. Recipes are in [surfaces.md](surfaces.md#shadow-recipes).

## Outline images in pure black or white

Give content images a `1px` outline inset by `1px`. Use `oklch(0 0 0 / 0.1)` in light mode and `oklch(1 0 0 / 0.1)` in dark. Never use a palette near-black, a tinted neutral or the accent color, because the tint shows as a colored fringe on the image edge. Skip transparent artwork such as logos and illustrations. The recipe is in [surfaces.md](surfaces.md#image-outlines).

## Transitions, not keyframes, for interactive state

Drive interactive state changes with CSS transitions or a motion library's springs, which retarget when the user reverses mid-flight. Keyframes run a fixed timeline and cannot reverse, so reserve them for sequences that run once. See [animations.md](animations.md).

## Press scales to 0.96

A pressed button scales to `0.96` over `150ms` with `ease-out`, and a disabled one never scales. Give the button an opt-out where the motion would distract, through the component's existing variant API or a `static` prop. See [recipes for CSS, Tailwind and Motion](animations.md#scale-on-press).

## High-frequency interactions respond instantly

Keystrokes, row hovers and tab switches get instant feedback, or a transition of `150ms` or less on `opacity` or `background-color`. Reserve expressive motion for infrequent moments such as a view's first load, a success state or an empty state.

Every animated state change also leaves a static cue in the form of a color, icon or label. Motion is never the only feedback channel.

## Gate motion behind the reduced-motion preference

`better-accessibility` owns the requirement. Recipes in the reference files show the full-motion form, so add this gate and the query in **Hover effects only on hover-capable pointers** when you use one. Run movement, blur and entrance or exit scale only under `prefers-reduced-motion: no-preference`. The press scale stays, because it is brief feedback and not travel. Under reduced motion, replace them with an opacity cross-fade rather than removing the element instantly. Recipes for CSS, Tailwind and Motion are in [animations.md](animations.md#reduced-motion-fallback).

## Stagger infrequent entrances by 100ms

Where sequence communicates hierarchy in a staged entrance, split the content into semantic chunks such as title, description and actions. Stagger them `100ms` apart, each entering with opacity, `4px` of blur and `12px` of `translateY` over `300ms` with `ease-out`. Never stagger routine interactions. See [enter-exit.md](enter-exit.md).

## Exits are shorter and smaller than enters

Exit with opacity, `4px` of blur and a fixed `-12px` `translateY` over `150ms` with `ease-out`, never the full container height. Slide fully out only where the destination carries meaning, such as a drawer closing. Remove the element instantly where motion adds no information. See [enter-exit.md](enter-exit.md#exit-animations).

## Skip state animations on first render

Set `initial={false}` on an `AnimatePresence` that wraps a state swap, so the default state does not animate in on mount. Never set it around an intentional entrance such as a staggered hero. See [animations.md](animations.md#skip-animation-on-page-load).

## Cross-fade contextual icons with exact values

Where an icon swaps on an infrequent state change, such as play to pause or copy to copied, cross-fade it. Scale runs `0.25` to `1`, opacity `0` to `1` and blur `4px` to `0px`. With a motion library, use `{ type: "spring", duration: 0.3, bounce: 0 }`, and bounce is always `0`. Without one, keep both icons in the DOM and cross-fade over `300ms` with `cubic-bezier(0.2, 0, 0, 1)`.

A tab's icon swap and the actions a row reveals on hover fall under **High-frequency interactions respond instantly**. Both recipes are in [icon-transitions.md](icon-transitions.md).

## Suppress transitions on theme switch

Disable every transition for the theme swap, force a style flush and restore transitions after the next frame. Otherwise every color transition fires at once and the switch smears. See the [recipe](animations.md#suppress-transitions-on-theme-switch).

## Transition only what changes

Name the exact properties, as in `transition-property: scale, opacity`, and never `all`. See [performance.md](performance.md).

## Name the animated property in will-change

Add `will-change` only after you see first-frame stutter, and name the property you animate: `scale` for `scale`, `transform` for `transform`. Limit it to transform properties, `opacity` and `filter`, and never use `all`. See [performance.md](performance.md#use-will-change-sparingly).

## Hover effects only on hover-capable pointers

Put hover-only styling behind `@media (hover: hover)`. On touch, `:hover` latches after a tap and reads as a stuck selected state. Tailwind 4's `hover:` variant already compiles under that query, and Tailwind 3's does not. Where a control draws its own pressed state, set `-webkit-tap-highlight-color: transparent` so the default gray flash does not double it.

## Contain scroll inside overlays

Give scrollable dialogs, drawers, menus and side panels `overscroll-behavior: contain`, so scrolling past their end never scrolls the page behind.

## Match icon stroke to text weight

An icon's rendered stroke tracks the weight of the text beside it, from `1.5px` at 400 to `2.5px` at 700. Use one icon library per surface. The table, sizing and grid rules are in [icons.md](icons.md).

## One SVG, recolored per state

Icons use `currentColor` and take hover, selected and disabled states from CSS color and opacity, never from separate assets. Outline is the default variant, and fill marks the active state. Under `dir="rtl"`, mirror only icons whose meaning follows reading direction. See [icons.md](icons.md#icons-in-rtl).

## Before you finish

| Detection | Fix |
| --- | --- |
| Padded parent and child with the same `rounded-*` or `border-radius` | Add the padding to the outer radius |
| `box-shadow: 0 0 0 1px` ring with no `border` beside it | `border: 1px solid transparent` |
| `outline-slate-*`, `outline-zinc-*` or a hex outline color on `img` | `outline-black/10` and `dark:outline-white/10` |
| `transition-all` or `transition: all` | Name the changing properties |
| `animation:` set on `:hover`, `.open` or another toggled class | A transition on the same properties |
| `active:scale-95`, `scale-90` or `whileTap` below `0.96` | `0.96` |
| `:active` scale with no `:disabled` exclusion | `enabled:active:` or `:not(:disabled):active` |
| `bounce:` set to anything but `0` | `bounce: 0` |
| `opacity: 0` in a base rule that only an `animation` reveals | Hidden state in the `from` keyframe, `animation-fill-mode: both` |
| `translate`, `filter`, `motion.*` or a `scale` other than the press with no `prefers-reduced-motion`, `motion-safe:` or `MotionConfig` guard | Gate it and cross-fade opacity under reduced motion |
| Color transitions plus a theme toggle with no `disableTransitionOnChange` or override | The theme-switch suppression recipe |
| `will-change: all`, or `will-change` on an element that never animates | Name the animated property or delete it |
| `fill="#..."` or `stroke="#..."` inside an icon SVG | `currentColor` |
| Plain-CSS `:hover` or Tailwind 3 `hover:` outside `@media (hover: hover)` | Wrap it in the query |
| `overflow: auto` or `overflow-y-auto` on a dialog, drawer or menu with no `overscroll-behavior` | `overscroll-behavior: contain` |

## Reporting

**Verification.** Without a browser, read every state the component defines from the code, such as hover, pressed, selected, loading and empty, with its durations and easings. With one, walk each state and replay motion at 10% speed in the browser's Animations panel. Report every check you could not run as `Not verified`.

A standalone review takes its severity ladder and format from [reporting.md](reporting.md).
