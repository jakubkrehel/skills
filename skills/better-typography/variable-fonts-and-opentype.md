# Variable fonts and OpenType

What a font file can do beyond drawing letters and how to reach those abilities from CSS.

## Static vs variable

A variable font is not automatically better. At one or two weights, static files can be smaller. At several weights, optical sizes or custom axes, a variable font usually wins.

## Disable synthesis narrowly

`font-synthesis: none` disables weight, style, small-cap and position synthesis together. Scope it to an isolated treatment you have verified across the fallback stack:

```css
.brand-wordmark {
  font-synthesis: none;
}
```

To disable one mode, use its longhand: `font-synthesis-weight`, `font-synthesis-style`, `font-synthesis-small-caps` or `font-synthesis-position`.

## Axes

A font supports only the axes its designer included. Inter's variable file exposes only `wght` and `opsz`, and Roboto Flex adds custom ones such as `GRAD`.

Optical sizes predate variable fonts. Many families still ship them as separate `Text` and `Display` files instead of an `opsz` axis.

## Raw tags only for custom axes

`font-variation-settings` silently does nothing on a non-variable fallback, where `font-weight` still picks the nearest face:

```css
/* Good: common axes use the properties */
.heading {
  font-weight: 650;
}

/* Good: custom axis with no property of its own */
.heading-grade {
  font-variation-settings: "GRAD" 80;
}

/* Bad: weight via raw tag breaks on fallback fonts */
.heading {
  font-variation-settings: "wght" 650;
}
```

## OpenType features

Unlike axes, features work the same on static and variable fonts. A font ships only the features its designer included.

Common features have a `font-variant-*` property, and `font-feature-settings` is for the rest:

```css
/* Good: common features use the properties */
.price {
  font-variant-numeric: tabular-nums;
}

.id {
  font-variant-numeric: slashed-zero;
}

/* Good: niche feature with no property of its own */
.logo {
  font-feature-settings: "ss01" 1;
}
```

## Small caps, superscripts, subscripts

Enable real small capitals with `font-variant-caps` and drawn superscripts and subscripts with `font-variant-position`. Both require the font to include the glyphs.

## Stylistic sets and character variants

`ss01` = stylistic set, slot 01. `cv11` = character variant, slot 11. What each slot does differs font to font, which is why they are numbered, not named. Check the font's docs. In Inter, `ss01` switches to open digits and `cv11` swaps in a single-story `a`.
