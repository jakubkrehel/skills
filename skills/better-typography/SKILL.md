---
name: better-typography
description: Sets and reviews how text renders, from the type scale and spacing to font features, wrapping, truncation and punctuation.
---

# Typography

This skill sets and reviews how text renders, from the type scale and spacing to font loading, wrapping and punctuation. It writes every fix in the project's styling system, and the [cheat sheet](css-cheat-sheet.md) gives the Tailwind form of each declaration with no obvious utility.

The words belong to `better-writing`, semantic heading structure to `better-accessibility` and spatial RTL layout to `better-layout`. Measure contrast with `better-colors`; whether it passes is `better-accessibility`'s.

## Measured, not preferred

Some values here are exact. Unitless line-height, weight `400` or heavier below `18px`, a 60–75 character measure, `16px` inputs on iOS and `tabular-nums` on changing values are findings when missed.

Letter-spacing, pairing and scale ratios are heuristics. Report them only where they break the project's own scale. Never propose a new typeface, paid or free, unless the task asks for a type change.

## Fewer fonts, sizes and weights

Rarely use more than three fonts; marketing pages can carry more than apps. Weight and size carry hierarchy, so each extra weight or size dilutes it. Pair for contrast. A serif headline over a sans body reads as deliberate, two near-identical sans-serifs read as a mistake.

Below `18px`, use weight `400` or heavier. Weights `100`–`300` belong at `28px` and up, checked against the background even there. `Display` and `Text` cuts, formats and fallback stacks are in [choosing-fonts.md](choosing-fonts.md).

## Load the faces the design uses

Browsers synthesize a weight or style the family doesn't ship, distorting the face. Load every face the design uses. Disable synthesis only after verifying the fallback stack, and only the unwanted mode. The [synthesis longhands](variable-fonts-and-opentype.md#disable-synthesis-narrowly) show how.

## Properties over raw tags

When a CSS property exists, use it. `font-weight: 650`, not `font-variation-settings: "wght" 650`. `font-variant-numeric: tabular-nums`, not `font-feature-settings: "tnum" 1`. Properties keep working when a non-variable fallback renders.

Leave `font-optical-sizing` at its default `auto`. Hard-code `"opsz"` only when the design deliberately decouples optical size from font size. Reserve raw tags for custom axes like `"GRAD" 80` and features with no property, like `"ss01" 1`. Axes, features and variable versus static files are in [variable-fonts-and-opentype.md](variable-fonts-and-opentype.md).

## Use a type scale with semantic names

Define a small set of sizes and deviate from it as little as possible. Pair each size with its line-height and weight, so a role is one decision instead of three.

Solo, default names like `text-sm` are fine when the usage rules are clear. On a team, name sizes by use (`text-body-sm`) so the rules survive other people. A [role-based starting scale](spacing-and-sizing.md#type-scale) is in the reference.

## Heading sizes descend with level

Map heading levels to descending steps of the scale, so a subordinate heading never overpowers its parent. Deep levels may share a size where the scale runs out of steps, as long as weight or letter-spacing keeps them distinct. A heading is never smaller than body text unless it is a deliberate overline. See the [level mapping](spacing-and-sizing.md#heading-hierarchy).

## Line-height by role

Display text `1.1`, headings `1.2`–`1.3`, body copy `1.5`–`1.6`. Use unitless values so line-height scales with the font size; a fixed `24px` does not.

Tight line-height is for short text. Anything that wraps to three or more lines needs at least `1.4`, even in a height-constrained row. See the [card description example](spacing-and-sizing.md#line-height-for-wrapping-text).

## Letter-spacing by size

Headings at `24px` and up take `-0.01em` to `-0.02em`. Uppercase labels at `14px` or smaller take `0.05em`. Body copy stays at `0`. Use `em` so tracking scales with the size.

Kerning is built into the font and on by default. `font-kerning: none` is never a fix.

## Cap the measure

Cap long-form text at 60–75 characters per line. Any unit works, as long as a cap exists and the line length lands in range. See [unit choices and the pixel equivalents](wrapping-and-punctuation.md#measure-line-length).

## Wrap deliberately

- `text-wrap: balance` evens out headings and short descriptions. Never on paragraphs, and Chromium ignores it past six lines.
- `text-wrap: pretty` keeps a lone word off the last line. Safe on any paragraph, long-form included.
- `overflow-wrap: break-word` where a long word, link or ID could escape the container.
- `white-space: nowrap` on labels and badges where a line break looks broken.

Keep interface text at `text-align: start`. `justify` belongs only in specific editorial layouts.

## Tabular numbers on changing values

Apply `font-variant-numeric: tabular-nums` to timers, counters, prices and numeric table columns, so every digit keeps one width and nothing shifts. Leave figures in prose proportional.

## Truncate with a way back

For one line, `overflow: hidden`, `text-overflow: ellipsis` and `white-space: nowrap`, which Tailwind's `truncate` sets together. For several, `display: -webkit-box`, `-webkit-box-orient: vertical`, `-webkit-line-clamp: 3` and `overflow: hidden`, which Tailwind's `line-clamp-3` emits.

A truncated value needs a way back to the full text, which `better-layout` owns.

## Natural case, typographic punctuation

Store text in natural case and set case with `text-transform`, so a redesign never means rewriting copy.

Rendered text uses typographic characters, never their keyboard stand-ins. Curly quotes, en dashes, the ellipsis character and non-breaking spaces are in the [substitution table](wrapping-and-punctuation.md#smart-punctuation). Code keeps straight quotes.

## Underlines from the font

Set `text-underline-position: from-font` and `text-decoration-thickness: from-font`, or tune `text-underline-offset` and thickness by hand. A dotted `text-decoration-style` hints that a word carries extra information, such as an abbreviation or a defined term. See the [underline recipes](details-and-accessibility.md#underlines).

Color is the only part of a real underline that animates reliably. Any other underline animation needs a separate element, and its motion is `better-ui`'s.

## Inputs at 16px on mobile

iOS Safari zooms the whole page when an input's text is smaller than `16px`. Two fixes hold `16px` and look different, so ask which one the design wants, or offer both in a review:

- Size the input up on mobile (`text-base sm:text-sm`). Changes how it looks on small screens.
- Keep `font-size: 16px` and render the intended size with `transform: scale()`. Identical at every viewport, more code to maintain.

Both recipes, plus placeholder and caret styling, are in [details-and-accessibility.md](details-and-accessibility.md#forms-and-editable-text).

## Size floors

Start long-form body text at `16px`, the browser default. Move off it only for a reason you can name, such as a typeface that runs small, a narrow measure or a dense professional tool.

UI text can go smaller. Start inputs and menus at `14px` and captions at `13px`, and rarely go below `12px`. Set `font-size` in `rem` so the reader's browser font size still applies.

## Font smoothing on the root

Apply `-webkit-font-smoothing: antialiased` and `-moz-osx-font-smoothing: grayscale` once on the root, never per component. Tailwind's `antialiased` sets both.

## Language and bidi behavior

Set `lang` so browsers and assistive technology pick the right quotes, hyphenation and pronunciation. Set `dir` on the document and wherever direction changes. Wrap a mixed-direction value in `<bdi>` and never reorder digits by hand. How multi-line paragraphs and numbers behave is in [wrapping-and-punctuation.md](wrapping-and-punctuation.md#internationalization).

## Trim text boxes in tight containers

In buttons and badges, the space a font reserves above and below its letters makes text sit low. `text-box: trim-both cap alphabetic` trims it as a progressive enhancement. See the [edges, keywords and support](spacing-and-sizing.md#text-trimming-with-text-box).

## Decorative text in CSS, not images

Build drop caps, gradient text, outlines and text shadows in CSS so the text stays selectable and searchable. The [properties and their support](details-and-accessibility.md#decorative-text) are in the reference.

## Keep useful text selectable

Keep text selectable by default. `::selection` can carry brand into the reading experience, as long as the selected combination stays legible. [Styling other ranges](details-and-accessibility.md#selection) such as search matches is in the reference.

`user-select: none` belongs on a draggable or gesture-driven surface where accidental selection interferes. Never across the interface and never because a button label can be highlighted.

## Before you finish

| Pattern | Fix |
| --- | --- |
| `font-weight` value with no matching weight in the loaded `@font-face` files | Load that face or use a weight the family ships |
| `font-synthesis: none` on body or interface text | Remove it; set only the unwanted longhand after checking the fallback stack |
| `font-variation-settings: "wght"` or `"opsz"` | `font-weight`; drop `"opsz"` so `font-optical-sizing: auto` applies |
| `font-feature-settings: "tnum"` or `"zero"` | `font-variant-numeric: tabular-nums` or `slashed-zero` |
| `line-height` in `px` or `rem` | A unitless value |
| `leading-none` or `leading-tight` on a description, card body or other text that wraps to three or more lines | `1.4` or more |
| `letter-spacing` in `px` | The same value in `em` |
| Body `font-size` in `px` | `rem` |
| Weight `100`–`300` or `font-thin`, `font-light` on text under `18px` | `400` or heavier |
| `text-wrap: balance` on a paragraph | `text-wrap: pretty` |
| `text-align: justify` in application UI | `text-align: start` |
| `-webkit-line-clamp` without `display: -webkit-box` | Add `display: -webkit-box` and `-webkit-box-orient: vertical` |
| A timer, counter, price or numeric column without `tabular-nums` | `font-variant-numeric: tabular-nums` |
| `...`, `--` or a hyphenated range in rendered strings | `…`, an em dash or an en dash |
| `text-decoration-skip-ink: none` | Remove it; set `text-underline-offset` or `from-font` instead |
| `<input>` with `text-sm` or `14px` and no mobile override | One of the **Inputs at 16px on mobile** fixes |
| `-webkit-font-smoothing` or `antialiased` inside a component | Move it to the root |
| Mixed-direction value such as a name, number or URL without `<bdi>` or `dir` | Wrap it in `<bdi>` |
| `user-select: none` or `select-none` on a layout root or text container | Remove it; keep it only on a drag or gesture surface |

## Reporting

**Verification.** Without a browser, check declared size and weight per heading level, descending within each semantic section. Check declared line-height, units and measure. Check truncation rules against realistic string lengths. With one, compare computed values, then resize the viewport with real content to catch wrapping, lone last-line words and truncation. Report every check you could not run as `Not verified`.

A standalone review takes its severity ladder and format from [reporting.md](reporting.md).
