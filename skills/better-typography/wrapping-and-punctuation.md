# Wrapping and punctuation

Line length, typographic characters and mixed-direction text.

## Measure (line length)

Any unit works. One `ch` is the width of the `0` in the current font, which is wider than the average character, so 60–75 characters is about `45ch`–`60ch`. At a `16px` body size the range lands roughly between `420px` and `570px` depending on the font. Tailwind's `max-w-md` (`448px`) and `max-w-lg` (`512px`) fit. `max-w-xl` (`576px`) runs a few characters over, and `max-w-prose` (`65ch`) holds 80 or more. Recheck the cap if the body font size changes.

## Smart punctuation

| Instead of | Use |
| --- | --- |
| Straight quotes `"..."` | Curly quotes that curve around the text (keep straight quotes in code) |
| Hyphen in ranges | En dash: `2010–2020` |
| Two hyphens for an aside | Em dash character |
| Three periods `...` | The single ellipsis character `…` |
| Regular space in `10 km` | `&nbsp;` so the value never breaks apart |
| Uncontrolled word breaks | `&shy;` to mark where a word may break, or `hyphens: auto` with the correct `lang` |

## Internationalization

- **Long paragraphs align by their own language.** A one- or two-line snippet follows the surrounding UI's direction. A paragraph of three or more lines aligns to its own script instead, so an English paragraph stays start-aligned LTR even inside an RTL interface. `text-align: start` with the correct `lang` and `dir` on the paragraph element handles this.
- **Digits keep their order.** A phone number or "541" reads identically in RTL, because the Unicode bidi algorithm keeps digit runs left to right. `<bdi>` repairs a value whose order adjacent RTL text disturbs.
