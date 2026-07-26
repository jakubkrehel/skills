<a href="https://interfaces.dev/">
  <img width="320" height="168" alt="interfaces.dev" src="https://ho1jr3x2dcwdu3t5.public.blob.vercel-storage.com/interfaces-og-image.png" />
</a>

[![skills.sh](https://skills.sh/b/jakubkrehel/skills)](https://skills.sh/jakubkrehel/skills)

A collection of agent skills that help with various parts of building a great interface. From animation and UI polish to accessibility and product writing.

## Skills

- [**better-interface**](skills/better-interface/SKILL.md): A user-invoked, cross-discipline interface review that coordinates every skill below.
- [**better-ui**](skills/better-ui/SKILL.md): Design engineering details that make interfaces feel polished: border radius, shadows, animations and micro-interactions.
- [**better-typography**](skills/better-typography/SKILL.md): Web typography from choosing fonts to spacing, wrapping and accessibility.
- [**better-colors**](skills/better-colors/SKILL.md): OKLCH color space: palette generation, contrast, gamut handling and theming.
- [**better-accessibility**](skills/better-accessibility/SKILL.md): Focus states, keyboard support, ARIA, forms, screen readers, hit areas and motion.
- [**better-layout**](skills/better-layout/SKILL.md): Layout structure, grouping, alignment, reading order, progressive disclosure and adaptive breakpoints.
- [**better-writing**](skills/better-writing/SKILL.md): UX writing and interface copy, from button labels to errors, settings and empty states.
- [**should-i-animate-this**](skills/should-i-animate-this/SKILL.md): A motion audit that judges an animation against how often it fires, then keeps, shortens or cuts it.

## Install

```bash
npx skills add jakubkrehel/skills
```

You can choose which skills to install or install all of them. `better-interface` coordinates the other six skills, so install the complete collection when you want holistic reviews.

```bash
npx skills add jakubkrehel/skills --skill '*'
```

## Use

The default review mode is `full`. Pass `quick` for a shorter revie, and add the screen, flow, or feature after the mode.

In Claude Code:

```text
/better-interface
/better-interface quick
/better-interface full checkout flow
```

In Codex:

```text
$better-interface
$better-interface quick
$better-interface full checkout flow
```

`should-i-animate-this` is a separate, standalone audit. Point it at an animation, a component or a whole flow, and it returns a verdict per animation — keep, shorten, make one-sided, first-run only or cut.

```text
/should-i-animate-this
/should-i-animate-this the context menu
$should-i-animate-this the checkout flow
```
