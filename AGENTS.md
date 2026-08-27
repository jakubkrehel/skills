# AGENTS.md

This file is the single source of guidance for coding agents working in this repository. `CLAUDE.md` imports it and adds nothing but Claude Code specifics, so put repository facts here and do not maintain a second copy.

## What this repository is

A collection of agent skills for building great product interfaces (typography, colors, UI polish), distributed two ways: via `npx skills add jakubkrehel/skills` and as the Claude Code plugin `interfaces` served by the marketplace in this same repository. It is documentation-only; there is no build, lint, or test tooling.

`.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json` define the plugin and its marketplace. Both are named `interfaces`, so plugin users invoke skills as `/interfaces:better-interface` while skills-CLI users invoke `/better-interface`. Skills are discovered from `skills/` automatically, so adding a skill needs no manifest change. Bump `version` in `plugin.json` in the same commit as any change under `skills/`. That number is the only signal plugin users update on. `claude plugin update` compares it and nothing else, so a change shipped without a bump reports "already at the latest version" and never reaches them. Run `claude plugin validate .` and `claude plugin validate .claude-plugin/plugin.json` after touching either manifest.

`opencode.json` registers `skills/` under `skills.paths` so opencode loads the collection while this repository itself is open, which is for working on the skills rather than distributing them. opencode users install through the skills CLI's opencode target, and opencode exposes every discovered skill as a slash command on its own, so this repository carries no opencode command wrappers.

## Structure

Each skill lives in `skills/<skill-name>/`, with `SKILL.md` as the entry point and supporting `.md` files beside it.

Skills come in two shapes. A domain skill holds knowledge: what is true about typography, color, or layout. A verb skill holds a procedure: review this change, explore these variants, explain this interface. A procedure sitting inside a domain skill is a candidate for extraction, and a domain rule sitting inside a verb skill belongs to its owner instead.

The content headings belong to the skill, not to a house style: a set of files all filling one section template reads like instances of one file. What is shared is the small amount of framing that calibrates behaviour rather than organising content.

Every `SKILL.md` carries:

- **Frontmatter** with `name` (matching the directory) and `description`.
- **A plain-name H1** and a two-sentence opener saying what the skill is and what it does. Not what the domain means or why it matters: an agent does not need motivating, and a reader can tell the difference.
- **A calibration line or two**, in the opener or in its own section where it needs the room. This is where a skill says how hard to press: which values are exact rather than approximate, what counts as a finding versus a preference, when the right answer is to write nothing. A skill that lists rules without saying how hard to press leaves that to chance. That is the difference between a review that blocks on evidence and one that blocks on taste. Give the section a heading that carries its own point (`Evidence, not taste`), not a generic label.
- **Headings that carry the point**, in sentence case. `Native elements first`, not `Semantics`. Number them only where the steps genuinely run in order, as `better-interface` and `interface-review` do; numbering flat reference implies a sequence that isn't there and makes every insertion a renumber.
- **A hand-off line** naming the sibling skills that own adjacent topics.
- **A `## Before you finish` table**, two columns, where the domain has recurring mistakes. The left column is the detection pattern, which is what a principle statement does not give you. The heading names the moment on purpose: `Common mistakes` is a label an agent reads past while orienting, and `Before you finish` names the point in the work where the table is worth consulting.
- **A `## Reporting` section** in every domain skill, carrying that domain's severity ladder, its verification checks and the format for a standalone review. See below.

Supporting `.md` files carry depth beyond the principle statements: recipes, code patterns, lookup tables. Link each one from the principle that needs it, so the link sits where the agent lands. A principle states the rule and links out for the recipe. It never restates the reference file in shorter form, and the reference file never restates the principle in longer form.

Each rule lives in exactly one skill. Other skills point to it by skill name in backticks (`better-layout`), never by cross-skill relative link, because each skill directory ships on its own.

Point at a principle by its heading in bold (**Classify every finding**), never by its number. A numbered reference breaks silently the moment a principle is inserted above it, and nothing in the file fails when it does.

### The review format

Each skill carries the format for the review it produces. `better-interface` holds the orchestrated format in `review-format.md`: scope and coverage, the findings table, verification and the verdict. A domain skill's `## Reporting` holds the smaller standalone format, grouped by the principle each finding violates and with no `Domain` column. `interface-review` holds the change-scoped format, with its status column and its pre-existing section, and `better-interface` points at it rather than keeping a second copy.

Those three overlap, and that overlap is the price of a skill that works when installed alone. Someone who installs only `better-typography` has no `better-interface` on disk to read a format out of.

### Invocation

A user-invoked skill may invoke model-invoked skills, but it can never reach another user-invoked skill. That rule decides the setting; it is not a preference:

- `interface-review`, `variant`, `break` and `explain-interface` are the user-invoked skills. Each carries `disable-model-invocation: true` in its frontmatter **and** `policy.allow_implicit_invocation: false` in its `agents/openai.yaml`. Those are the Claude Code and Codex halves of the same switch and must be set together, or the skill behaves differently per harness. Their `description` is human-facing: a one-line summary with no trigger list, since nothing but a person can match against it.
- `variant` is user-invoked because a design exploration is never something to start on someone's behalf. It writes throwaway code and then asks a question only a person can answer, so an agent firing it unprompted produces work nobody asked for and a harness nobody deletes.
- `break` is user-invoked for the same reason as `variant`: it writes a throwaway harness, and an agent firing it after every component it touches would litter the repo with test pages.
- `explain-interface` is user-invoked because studying someone else's interface is only ever something a person asks for. Pasting a URL is not a request to analyse it, and a skill firing on every link would turn each mention of a site into a report.
- Every other skill is model-invoked and keeps a trigger list, because something must reach it: `better-interface` routes to every domain skill, and `interface-review` hands its review up to `better-interface`.
- `better-interface` therefore cannot start `interface-review`. Where it would want to, it asks the user to run it. Making `better-interface` user-invoked too would sever the upward handoff and force `interface-review` to restate severity, the cap, the format and the verdict.

### Rule ownership

| Skill | Owns |
| --- | --- |
| `better-interface` | Review orchestration, project convention discovery, shared severity and its escalation triggers, the shared remediation ordering, consolidation, coverage, the finding cap, the orchestrated output format and the verdict |
| `interface-review` | Change scope resolution including the empty-scope offer, blast radius from changed files to affected surfaces, finding classification (`Introduced` / `Regression` / `Pre-existing`) and the change-scoped report format |
| `variant` | Design exploration: the axis set variants may diverge on, how many to build, the harness and picker, the tradeoff table and promotion. Owns no domain rules; every variant clears `better-interface`'s escalation triggers as its floor |
| `break` | Robustness testing of one component: the scenario axes and their applicability cues, the harness page that doubles as the visual report, the render-and-observe rule and the survived / broke findings. Owns no domain rules and issues no verdict; each break names the domain skill that owns the fix. Isolates on purpose where `variant` insists on the real page |
| `explain-interface` | Reading an interface you did not build: scoping to the thing asked about, the layer search, the URL and screenshot branches, the measured / derived / inferred evidence tiers, treating fetched page content as untrusted input, explaining the mechanism rather than the readout, and the minimal reproduction. Owns no domain rules and issues no verdict; it names what it finds in each domain skill's vocabulary |
| `better-accessibility` | Semantic HTML, keyboard and focus behavior, accessible names, forms, assistive technology and accessibility requirements |
| `better-layout` | Spatial grouping, alignment, spacing, responsive structure, logical CSS properties and spatial RTL behavior |
| `better-writing` | Source wording, terminology, voice, tone, labels, errors and empty-state copy |
| `better-typography` | Visual text rendering, type systems, font behavior, wrapping mechanics, punctuation and text-level bidi behavior |
| `better-colors` | Palette structure and step roles, palette construction, color token naming, color notation, gamut, rendered-pair contrast measurement and color remediation |
| `better-ui` | Optional visual polish: surfaces, icons and motion aesthetics after the underlying interaction is sound |

When a concern crosses domains, keep the rule in the owner above and let other skills name only the handoff or secondary effect. In particular:

- `better-accessibility` decides when contrast is required and whether the pair fails; `better-colors` owns measuring the rendered pair and changing its colors. Severity is `better-interface`'s in an orchestrated review; each domain skill defines severity only for its own standalone output.
- `better-accessibility` owns semantic heading structure; `better-typography` owns how heading levels render visually.
- `better-layout` owns logical CSS properties and spatial mirroring; `better-typography` owns language metadata, punctuation and mixed-direction text.
- `better-typography` owns truncation mechanics; `better-layout` owns whether the surrounding layout has room or an expansion affordance; `better-writing` owns the source copy.
- `better-accessibility` owns reduced-motion requirements; `better-ui` owns the optional animation recipe used when motion is appropriate.
- `interface-review` owns what to review when the scope is a diff; `better-interface` owns how that review is routed, ranked, consolidated and reported. The dependency runs one way: `interface-review` hands its scope and statuses up, and `better-interface` ranks, caps and issues the verdict. Neither file may restate the other's rules.

## Authoring conventions

- Principles are prescriptive and specific: exact CSS properties, exact values (e.g. scale `0.25` → `1`, blur `4px` → `0px`), not vague advice.
- Match the degree of prescription to the decision: requirements may be unconditional, while design heuristics name the context and escape conditions before giving exact recipe values.
- Skills instruct agents to match the target project's existing styling system (Tailwind vs. plain CSS vs. CSS-in-JS) rather than impose one.
- Frontmatter `description` is how a skill gets found. Update its triggers whenever the skill's scope changes. It loads on every turn, so it earns harder pruning than the body: one trigger per distinct branch and never two phrasings of the same one. Leave out identity the body already carries, such as which skills it coordinates or which modes it supports.
- Skills that own a domain use the `better-*` prefix. A user-invoked review entry point may drop it when a plainer name reads better on the command line, as `interface-review` does.
- A skill's name appears in three places: its directory, its frontmatter `name` and `display_name` in its `agents/openai.yaml`. Renaming means changing all three, then `grep`ing for the old name to confirm nothing survived.
- Prefer counts and lists that cannot go stale. Say "every skill in this repository" rather than a number the next skill invalidates.
- Straight quotes, sentence-case headings. No em dashes and no parentheses or mid-sentence colons standing in for one: end the sentence or use a comma. En dashes are for numeric ranges only.
- No serial comma. Write `surfaces, icons and motion`, never `surfaces, icons, and motion`. The comma stays where `and` joins two clauses, and where dropping it would swallow an appositive or make the last two items read as one.

Four checks after an edit, since prose drifts back toward the mean:

- **No sentence over 30 words**, counting a code span as one word. A ceiling, not an average. Averages are the wrong instrument here: aiming for a low mean produces choppy prose, and the well-written references this collection was measured against average about 14 words with a long tail. What makes a file hard to read is the individual 40-word sentence carrying four clauses, so split those and leave the rest alone.
- **Around 20 triggers per description.** Two words for one branch is one branch written twice.
- **One statement of each rule.** Before adding a sentence, check whether the file already says it somewhere else. The reflex to restate a boundary "for clarity" produced four copies of one ownership line in `better-interface`. It also produced a mistake table whose every row repeated the principle above it.
- **A pruning pass, not a word ceiling.** No reference collection caps skill length; `anthropics/skills` runs to nearly 10,000 words in a single file. Read each sentence and ask what it changes. A sentence that cannot be restated as an instruction, a fact, or a number is cut. A sentence that could appear unchanged in another project's docs says nothing about this one. Prose about this repository's own filing decisions belongs in this file, never in a skill.
