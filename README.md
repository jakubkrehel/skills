<a href="https://interfaces.dev/">
  <img width="320" height="168" alt="interfaces.dev" src="https://ho1jr3x2dcwdu3t5.public.blob.vercel-storage.com/interfaces-og-image.png" />
</a>

[![skills.sh](https://skills.sh/b/jakubkrehel/skills)](https://skills.sh/jakubkrehel/skills)

A collection of agent skills that help with various parts of building a great interface. From animation and UI polish to accessibility and product writing.

## Skills

- [**better-interface**](skills/better-interface/SKILL.md): A user-invoked, cross-discipline interface review that coordinates every skill below.
- [**interface-review**](skills/interface-review/SKILL.md): A user-invoked review of your uncommitted changes, current branch or a pull request against every skill below.
- [**better-ui**](skills/better-ui/SKILL.md): Design engineering details that make interfaces feel polished: border radius, shadows, animations and micro-interactions.
- [**better-typography**](skills/better-typography/SKILL.md): Web typography from choosing fonts to spacing, wrapping and accessibility.
- [**better-colors**](skills/better-colors/SKILL.md): OKLCH color space: palette generation, contrast, gamut handling and theming.
- [**better-accessibility**](skills/better-accessibility/SKILL.md): Focus states, keyboard support, ARIA, forms, screen readers, hit areas and motion.
- [**better-layout**](skills/better-layout/SKILL.md): Layout structure, grouping, alignment, reading order, progressive disclosure and adaptive breakpoints.
- [**better-writing**](skills/better-writing/SKILL.md): UX writing and interface copy, from button labels to errors, settings and empty states.

## Install

### As a Claude Code plugin

Installs every skill in this repository together and updates in place. Run these inside Claude Code:

```text
/plugin marketplace add jakubkrehel/skills
/plugin install interfaces@interfaces
```

### With the skills CLI

Works in Claude Code, Codex and other agents. You can choose which skills to install or install all of them. `better-interface` coordinates the six domain skills and `interface-review` builds on `better-interface`, so install the complete collection when you want holistic or change-scoped reviews.

```bash
npx skills add jakubkrehel/skills
```

```bash
npx skills add jakubkrehel/skills --skill '*'
```

### With opencode

The repository is opencode-ready. Clone it and opencode discovers the skills automatically from the `skills/` directory (registered in the repo's `opencode.json`) plus the `/better-interface` and `/interface-review` slash commands from `.opencode/command/`.

For skills available in every opencode project on a machine, clone the repository and point the global config at it:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "skills": {
    "paths": ["/path/to/skills-repo/skills"]
  }
}
```

Add the same `skills.paths` entry to a project's `opencode.json` to share them within a single project, or copy any skill folder under `.opencode/skills/`. Restart opencode after changing the config; skills are loaded at startup.

## Use

Two skills are invoked by name. Use `better-interface` to review a screen, flow or feature, and `interface-review` to review what you changed: your uncommitted work, the current branch or a pull request.

The default review mode is `full`. Pass `quick` for a shorter review. For `better-interface`, add the screen, flow or feature after the mode. For `interface-review`, add the target after the mode; leave it off and it detects the branch or your uncommitted changes.

In Claude Code, as a plugin. Plugin skills are namespaced, so every skill is prefixed with `interfaces:`.

```text
/interfaces:better-interface
/interfaces:better-interface quick
/interfaces:better-interface full checkout flow
/interfaces:interface-review
/interfaces:interface-review quick pr 482
```

In Claude Code, installed with the skills CLI:

```text
/better-interface
/better-interface quick
/better-interface full checkout flow
/interface-review
/interface-review quick pr 482
```

In Codex:

```text
$better-interface
$better-interface quick
$better-interface full checkout flow
$interface-review
$interface-review quick pr 482
```

In opencode, as slash commands (provided by `.opencode/command/`, with the same argument forms as the Claude Code plugin):

```text
/better-interface
/better-interface quick
/better-interface full checkout flow
/interface-review
/interface-review quick pr 482
```

The prefix only affects skills you invoke by name. The six domain skills are picked up automatically from context either way.
