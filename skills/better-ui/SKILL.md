---
name: better-ui
description: Polishes surfaces and icons with exact values for border radius, optical alignment, shadows and icon states.
---

# UI polish

This skill holds the visual polish for surfaces and icons, with the exact value each detail takes. It applies once the underlying interaction is sound, and a polish finding never outranks a broken interaction.

## Exact values, optional polish

The values below are exact, not ranges to approximate. An outline at `oklch(0 0 0 / 0.1)` is not one at `0.15`, and a `1.5px` stroke is not `2px`. The optical nudges and the concentric padding cutoff are the exceptions. They are starting points, judged by eye.

Keep the project's component library, tokens and density. A deliberate and consistent project convention, such as a style with no shadows, is a preference and not a finding. The same detail done two ways within the project is a finding.

Text wrapping, font rendering, tabular numbers and text spacing belong to `better-typography`. Hit areas, keyboard support, ARIA and the reduced-motion requirement belong to `better-accessibility`. Grouping, section spacing, breakpoints and spatial RTL belong to `better-layout`, except directional icon mirroring. Color tokens and contrast measurement belong to `better-colors`. Transitions, enters and exits, icon swaps and every other animation belong to `better-motion`.

## Outer radius equals inner radius plus padding

Where nested surfaces share a visible, even inset, the outer radius is the inner radius plus the padding plus any border width. Past `24px` of padding, or where the padding is deliberately asymmetric, treat the layers as separate surfaces and keep each one's radius token. Recipes are in [surfaces.md](surfaces.md).

## Align optically where geometry looks off

Where geometric centering looks off, nudge by eye. Give a button `2px` less padding on its icon side, shift a play triangle toward its point and fix asymmetric glyphs in the SVG itself. Recipes are in [surfaces.md](surfaces.md#optical-alignment).

## Shadows for elevation, borders for structure

Where a border exists only to create depth, replace it with layered transparent `box-shadow` values. Keep borders on dividers, separators, table cells and selected states. Keep them on form inputs too, whose boundary needs 3:1 non-text contrast under `better-accessibility`. Focus rings belong to `better-accessibility` as well.

Forced-colors mode removes every `box-shadow`. Keep `border: 1px solid transparent` under a shadow ring so that mode still draws an edge. Recipes are in [surfaces.md](surfaces.md#shadow-recipes).

## Outline images in pure black or white

Give content images a `1px` outline inset by `1px`. Use `oklch(0 0 0 / 0.1)` in light mode and `oklch(1 0 0 / 0.1)` in dark. Never use a palette near-black, a tinted neutral or the accent color, because the tint shows as a colored fringe on the image edge. Skip transparent artwork such as logos and illustrations. The recipe is in [surfaces.md](surfaces.md#image-outlines).

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
| `fill="#..."` or `stroke="#..."` inside an icon SVG | `currentColor` |
| Plain-CSS `:hover` or Tailwind 3 `hover:` outside `@media (hover: hover)` | Wrap it in the query |
| `overflow: auto` or `overflow-y-auto` on a dialog, drawer or menu with no `overscroll-behavior` | `overscroll-behavior: contain` |

## Reporting

**Verification.** Without a browser, read every state the component defines from the code, such as hover, pressed, selected, loading and empty. With one, walk each state in both themes. Report every check you could not run as `Not verified`.

A standalone review takes its severity ladder and format from [reporting.md](reporting.md).
