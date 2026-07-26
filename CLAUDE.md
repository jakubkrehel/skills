# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A collection of agent skills for building great product interfaces (typography, colors, UI polish), published for installation via `npx skills add jakubkrehel/skills`. It is documentation-only; there is no build, lint, or test tooling.

## Structure

Each skill lives in `skills/<skill-name>/`:

- `SKILL.md` is the entry point. YAML frontmatter with `name` (matching the directory) and `description` (one-line summary, "Use when..." guidance, and a "Triggers on ..." keyword list that agents match against). The body: a short philosophy paragraph (one or two lines, with hand-off lines naming sibling skills that own adjacent topics), a **Quick Reference** table linking to reference files (only when the skill has them), numbered **Core Principles**, a **Common Mistakes** table, and a **Review Output Format** section. No review checklists and no trailing reference-file index; the Quick Reference is the only file listing.
- Supporting `.md` reference files are optional; single-file skills are fine. Add one only when it carries depth beyond the principle statements (recipes, code patterns, lookup tables), not to restate SKILL.md in longer form. Link via relative paths from the Quick Reference table.
- Each rule lives in exactly one skill; other skills point to it by skill name in backticks (e.g. `better-layout`), never via cross-skill relative links.

Current skills: `better-interface` (user-invoked cross-discipline review), `better-ui` (interface polish details), `better-typography` (web typography), `better-colors` (OKLCH color space and color usage), `better-accessibility` (accessibility engineering), `better-layout` (layout structure), `better-writing` (UX writing and interface copy), `should-i-animate-this` (user-invoked motion audit).

### Rule ownership

| Skill | Owns |
| --- | --- |
| `better-interface` | Review orchestration, shared severity, consolidation, coverage, and final output |
| `better-accessibility` | Semantic HTML, keyboard and focus behavior, accessible names, forms, assistive technology, and accessibility requirements |
| `better-layout` | Spatial grouping, alignment, spacing, responsive structure, logical CSS properties, and spatial RTL behavior |
| `better-writing` | Source wording, terminology, voice, tone, labels, errors, and empty-state copy |
| `better-typography` | Visual text rendering, type systems, font behavior, wrapping mechanics, punctuation, and text-level bidi behavior |
| `better-colors` | Color notation, palette construction, gamut, rendered-pair contrast measurement, and color remediation |
| `better-ui` | Optional visual polish: surfaces, icons, and motion aesthetics after the underlying interaction is sound |
| `should-i-animate-this` | Whether an animation belongs at all: frequency cost, critical-path impact, and motion removal |

`better-interface` orchestrates the six domain skills above it. `should-i-animate-this` is a standalone, user-invoked audit and is not part of that review.

When a concern crosses domains, keep the rule in the owner above and let other skills name only the handoff or secondary effect. In particular:

- `better-accessibility` decides when contrast is required and the severity of a failure; `better-colors` owns measuring the rendered pair and changing its colors.
- `better-accessibility` owns semantic heading structure; `better-typography` owns how heading levels render visually.
- `better-layout` owns logical CSS properties and spatial mirroring; `better-typography` owns language metadata, punctuation, and mixed-direction text.
- `better-typography` owns truncation mechanics; `better-layout` owns whether the surrounding layout has room or an expansion affordance; `better-writing` owns the source copy.
- `better-accessibility` owns reduced-motion requirements; `better-ui` owns the optional animation recipe used when motion is appropriate.
- `should-i-animate-this` decides whether an animation belongs and what it costs; `better-ui` owns how it is built once it belongs. Reduced motion is never the remedy for a cost problem.

## Authoring conventions

- Principles are prescriptive and specific: exact CSS properties, exact values (e.g. scale `0.25` → `1`, blur `4px` → `0px`), not vague advice.
- Match the degree of prescription to the decision: requirements may be unconditional, while design heuristics name the context and escape conditions before giving exact recipe values.
- Skills instruct agents to match the target project's existing styling system (Tailwind vs. plain CSS vs. CSS-in-JS) rather than impose one.
- Frontmatter `description` is the discovery surface; when adding or changing a skill's scope, update its trigger keywords accordingly.
- Domain skill directory names use the `better-*` prefix. A user-invoked audit skill may instead be named as the question it answers (e.g. `should-i-animate-this`) when that phrasing is how someone would invoke it. Renaming a skill means renaming its directory and frontmatter `name` together.
