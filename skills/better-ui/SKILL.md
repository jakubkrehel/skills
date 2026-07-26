---
name: better-ui
description: Design engineering principles for making interfaces feel polished. Use when building UI components, reviewing frontend code, implementing animations, hover states, shadows, borders, micro-interactions, enter/exit animations, choosing or reviewing icons, or any visual detail work. Triggers on UI polish, design details, "make it feel better", "feels off", stagger animations, border radius, optical alignment, image outlines, box shadows, icons, icon stroke weight, icon states.
---

# Details that make interfaces feel better

Great interfaces rarely come from a single thing. It's usually a collection of small details that compound into a great experience. Apply these principles when building or reviewing UI code.

When reviewing, slow the interface down: replay motion at 10% speed in the browser's Animations panel and walk every state: hover, focus, active, loading, empty. What feels off at 10% speed is what's subtly wrong at full speed.

Preserve the project's component library, tokens, and density. Match its established motion language except where a principle below prescribes an exact interaction pattern.

Typography (text wrapping, font rendering, tabular numbers, spacing) is covered by the `better-typography` skill; use that for anything text-related. Accessibility (hit areas, focus states, keyboard support, ARIA, reduced motion) is covered by the `better-accessibility` skill. Layout structure (grouping, spacing between sections, breakpoints, spatial RTL) is covered by the `better-layout` skill. This skill assumes an animation belongs; whether it belongs at all, and what it costs the user at its trigger frequency, is covered by the `should-i-animate-this` skill.

## Quick Reference

| Category | When to Use |
| --- | --- |
| [Surfaces](surfaces.md) | Border radius, optical alignment, shadows, image outlines |
| [Animations](animations.md) | Interruptible animations, enter/exit transitions, icon animations, scale on press |
| [Icons](icons.md) | Icon stroke weight, states via `currentColor`, outline vs fill, sizing, RTL flipping |
| [Performance](performance.md) | Transition specificity, `will-change` usage |

## Core Principles

### 1. Concentric Border Radius

Outer radius = inner radius + padding. Mismatched radii on nested elements is the most common thing that makes interfaces feel off.

### 2. Optical Over Geometric Alignment

When geometric centering looks off, align optically. Buttons with icons, play triangles, and asymmetric icons all need manual adjustment.

### 3. Shadows for Elevation, Borders for Structure

For buttons, cards, and containers whose border exists only to create depth, prefer layered transparent `box-shadow` values. Keep borders that communicate structure or state: dividers, layout separators, and selected or focus states.

### 4. Interruptible Animations

Use CSS transitions for interactive state changes: they can be interrupted mid-animation. Reserve keyframes for staged sequences that run once.

### 5. Split and Stagger Enter Animations

For an infrequent staged entrance where sequence helps communicate hierarchy, break content into semantic chunks and stagger them by ~100ms instead of animating one container. Use `should-i-animate-this` to confirm the entrance is infrequent enough to earn a stagger.

### 6. Subtle Exit Animations

Use a small fixed `translateY` instead of full height. Exits should be softer than enters. Use `ease-out` for both enter and exit transitions.

### 7. Contextual Icon Animations

Animate icons with `opacity`, `scale`, and `blur` instead of toggling visibility. Use exactly these values: scale from `0.25` to `1`, opacity from `0` to `1`, blur from `4px` to `0px`. If the project has `motion` or `framer-motion` in `package.json`, match that package's import path (or the established nearby imports when both exist) and use `transition: { type: "spring", duration: 0.3, bounce: 0 }`; bounce must always be `0`. If no motion library is installed, keep both icons in the DOM (one absolute-positioned) and cross-fade with CSS transitions using `cubic-bezier(0.2, 0, 0, 1)`; this gives both enter and exit animations without any dependency.

### 8. Image Outlines

Add a subtle `1px` outline with low opacity to images for consistent depth. The color must be pure black in light mode (`oklch(0 0 0 / 0.1)`) and pure white in dark mode (`oklch(1 0 0 / 0.1)`), never a near-black like slate, zinc, or any tinted neutral. A tinted outline picks up the surface color underneath it and reads as dirt on the image edge.

### 9. Scale on Press

A subtle `scale(0.96)` on click gives buttons tactile feedback. Always use `0.96`. Never use a value smaller than `0.95`: anything below feels exaggerated. Add a `static` prop to disable it when motion would be distracting.

### 10. Skip Animation on Page Load

Use `initial={false}` on `AnimatePresence` to prevent enter animations on first render. Verify it doesn't break intentional entrance animations.

### 11. Never Use `transition: all`

Always specify exact properties: `transition-property: scale, opacity`. Tailwind's `transition-transform` covers `transform, translate, scale, rotate`.

### 12. Use `will-change` Sparingly

Only for `transform`, `opacity`, `filter`, the properties the GPU can composite. Never use `will-change: all`. Only add when you notice first-frame stutter.

### 13. Match Icon Stroke to Text Weight

An icon next to text carries the text's optical weight: `1.5px` stroke beside regular (400) text, `2px` beside semibold (600). One stroke weight per icon set; never mix libraries on one surface.

### 14. One SVG, Recolored per State

Icons use `currentColor` and get their states (hover, selected, disabled) from CSS color and opacity, never from separate assets. Outline variant is the default; fill variant marks the active state.

## Common Mistakes

| Mistake | Fix |
| --- | --- |
| Same border radius on closely nested parent and child | Calculate `outerRadius = innerRadius + padding` |
| Icons look off-center | Adjust optically with padding or fix SVG directly |
| Border used only to fake elevation | Use layered `box-shadow` with transparency; keep structural and state borders |
| Jarring staged entrance or contextual exit | Stagger infrequent entrances and keep context-preserving exits subtle |
| Stateful icon or toggle animates its default state on page load | Add `initial={false}` to that `AnimatePresence`; preserve intentional page entrances |
| `transition: all` on elements | Specify exact properties |
| First-frame animation stutter | Add `will-change: transform` (sparingly) |
| Hairline icon beside bold text | Match the stroke width to the text weight |
| Separate icon assets per state | One `currentColor` SVG, states via CSS |
| Filled icons everywhere | Outline as default, fill only for the active state |

## Review Output Format

Use this format only when the user asks for a standalone UI-polish review. When `better-interface` orchestrates the review, provide domain evidence and findings to that skill and let its output format, severity scale, consolidation rules, cap, and verdict take precedence.

Present the standalone review in two parts.

### Findings

Group all confirmed findings by principle. Use a markdown table with **Severity**, **Location**, **Before**, **After**, and **Why** columns. Never use separate "Before:" / "After:" lines.

- **Severity**: `HIGH` makes an interaction misleading, unresponsive, or repeatedly disruptive; `MEDIUM` creates a noticeable craft or consistency problem; `LOW` is isolated polish.
- **Location**: cite `path/to/file:line`. If the artifact has no source files, cite the exact screen and component instead.
- **Before / After**: show the current implementation and an actionable replacement.
- **Why**: name the violated principle and explain how it affects the interface.

Consolidate a repeated systemic issue into one row and list every affected location. Omit principles with no findings.

### Example

#### Concentric border radius
| Severity | Location | Before | After | Why |
| --- | --- | --- | --- | --- |
| LOW | `src/Card.tsx:28` | `rounded-xl` on card + `rounded-xl` on inner button (`p-2`) | `rounded-2xl` on card (`8 + 8 = 16`), `rounded-lg` on inner button | Nested corners should be concentric |
| LOW | `src/card.css:11` | `border-radius: 16px` on both nested surfaces | Outer `24px`, inner `16px` with `8px` padding | Equal nested radii make the inner surface look pinched |

#### Scale on press
| Severity | Location | Before | After | Why |
| --- | --- | --- | --- | --- |
| LOW | `src/Button.tsx:19` | `<button className="...">` | Add `active:scale-[0.96] transition-transform` | Press feedback makes the control feel responsive |
| MEDIUM | `src/button.css:24` | `scale(0.9)` on press | Raise to `scale(0.96)` | Anything below `0.95` feels exaggerated |

### Verification and Verdict

After the findings:

1. **Verification**: list the exact checks run and their observed results. Walk every relevant state and inspect motion at 10% speed when animation is involved. If a check was not run, state what still needs verification.
2. **Verdict**: `Block` if any `HIGH` finding remains, `Needs changes` if only `MEDIUM` or `LOW` findings remain, and `Approve` only when no actionable findings remain.

When there are no findings, omit the tables, state "No actionable UI-polish findings", report verification, and end with `Approve`.
