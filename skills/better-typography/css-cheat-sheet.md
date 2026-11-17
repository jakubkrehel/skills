# CSS cheat sheet

Tailwind 4 equivalents for the declarations in this skill whose utility is not the obvious one. These are the compound utilities and the properties that only an arbitrary value reaches. Every other declaration here has a named utility.

## One utility, several declarations

| Declaration | Tailwind |
| --- | --- |
| `-webkit-font-smoothing` + `-moz-osx-font-smoothing` | `antialiased` |
| `overflow: hidden` + `text-overflow: ellipsis` + `white-space: nowrap` | `truncate` |
| `-webkit-line-clamp` + `display: -webkit-box` + `-webkit-box-orient: vertical` + `overflow: hidden` | `line-clamp-*` |
| `max-width` capping text at 60–75 characters | `max-w-md` / `max-w-lg` |

## Arbitrary values only

| Declaration | Tailwind |
| --- | --- |
| `font-synthesis-weight: none` | `[font-synthesis-weight:none]` |
| `font-variation-settings` | `[font-variation-settings:"GRAD"_80]` |
| `font-variant-caps` | `[font-variant-caps:small-caps]` |
| `font-variant-position` | `[font-variant-position:super]` |
| `text-box: trim-both cap alphabetic` | `[text-box:trim-both_cap_alphabetic]` |
| `text-underline-position: from-font` | `[text-underline-position:from-font]` |
| `-webkit-text-stroke` | `[-webkit-text-stroke:1px_black]` |
| `initial-letter` | `[initial-letter:3]` |

`text-decoration-thickness: from-font` is the exception with a named utility, `decoration-from-font`.
