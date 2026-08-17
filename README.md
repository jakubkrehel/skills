<a href="https://interfaces.dev/">
  <img width="320" height="168" alt="interfaces.dev" src="https://ho1jr3x2dcwdu3t5.public.blob.vercel-storage.com/interfaces-og-image.png" />
</a>

[![skills.sh](https://skills.sh/b/jakubkrehel/skills)](https://skills.sh/jakubkrehel/skills)

A collection of agent skills that help you build a great interface. They cover UI, typography, colors, accessibility, layout, product writing and more.

## Skills

- [**better-interface**](skills/better-interface/SKILL.md): A cross-discipline interface review that coordinates every skill below.
- [**interface-review**](skills/interface-review/SKILL.md): A user-invoked review of your uncommitted changes, current branch or a pull request against every skill below. Run it by name; it never starts on its own.
- [**better-ui**](skills/better-ui/SKILL.md): Design engineering details that make interfaces feel polished: border radius, shadows, animations and micro-interactions.
- [**better-typography**](skills/better-typography/SKILL.md): Web typography from choosing fonts to spacing, wrapping and accessibility.
- [**better-colors**](skills/better-colors/SKILL.md): Color systems: building and naming palettes, applying color with meaning, contrast and theming.
- [**better-accessibility**](skills/better-accessibility/SKILL.md): Focus states, keyboard support, ARIA, forms, screen readers, hit areas and motion.
- [**better-layout**](skills/better-layout/SKILL.md): Layout structure, grouping, alignment, reading order, progressive disclosure and adaptive breakpoints.
- [**better-writing**](skills/better-writing/SKILL.md): UX writing and interface copy, from button labels to errors, settings and empty states.
- [**anti-ui-slop**](skills/anti-ui-slop/SKILL.md): A finish gate for specific, complete, and functional interfaces instead of generic generated UI.

## Install

### CLI

Works in Claude Code, Codex, Opencode and other agents. You can choose which skills to install or install all of them.

```bash
npx skills add jakubkrehel/skills
```

### Claude Code plugin

Installs every skill in this repository together and updates in place. Run these inside Claude Code:

```text
/plugin marketplace add jakubkrehel/skills
/plugin install interfaces@interfaces
```
