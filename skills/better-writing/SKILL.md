---
name: better-writing
description: Writes and reviews interface copy, from labels and errors to empty states and confirmations, so it matches the product's voice and tells people what to do next.
---

# Writing

This skill writes and reviews interface copy such as labels, errors, empty states and confirmations, along with the terms that run through them. It matches the product's existing voice and flags copy that misleads people or leaves them without a way forward.

A deliberate brand voice is not a defect. Raise a departure from plain language only when it creates inconsistency, ambiguity, translation risk or a tone the stakes don't support. Rewording that merely suits your taste is not a finding.

How copy renders, including `text-transform`, truncation and smart punctuation, belongs to `better-typography`. Error markup, announcements and the attributes that carry accessible names belong to `better-accessibility`. Room for translated strings belongs to `better-layout`.

## Inventory the existing strings first

Before writing or reviewing, find where the copy lives and read the copy around the change:

1. Search for the translation call (`t(`, `i18n.`, `<FormattedMessage`, `$t(`) and the locale files it reads, such as `locales/**/*.json`, `messages/*.json`, `*.po` and `*.strings`. Copy may also come from a CMS or the API.
2. List the noun used for each object and the verb used for each action, as in "project" or "workspace" and "Delete" or "Remove".
3. Note the case used per element type and any voice or content style guide.

New copy uses the terms on that list. A synonym is a finding only where the same thing is named two ways.

## One voice, one vocabulary

The product has one voice and its existing copy establishes it. A local edit does not get to invent a new one. If it's "Archive" in the menu, it isn't "Move to storage" in the toast.

A multi-step flow uses one vocabulary throughout: "Get started" to enter, either "Continue" or "Next" to advance, "Done" to finish. Tone flexes with the stakes:

| Context | Tone |
| --- | --- |
| Success, onboarding, empty states | Warm, can be light |
| Routine actions, settings | Neutral, minimal |
| Errors, destructive confirmations | Calm, plain, zero playfulness |
| Data loss, security | Serious, explicit |

## Address the reader directly

In instructional copy, write "you", not "the user". In errors, "we" reads as deflection, so prefer "Unable to load content. Check your connection and try again." An established first-person voice can stay in low-stakes copy where it still reads clearly.

Use possessives sparingly: "Favorites" beats "Your favorites". Never mix perspectives in one flow, such as "My account" beside "Your settings".

## Plain words over clever ones

Choose words a tired reader gets on the first pass, and delete every word that does no work. No idioms, no colloquialisms and no humor that won't translate.

Skip unnecessary gender: "Subscribers can post recipes", not "each subscriber can post his or her recipes". Match the input device: "tap" on touch, "click" with a pointer, "select" when both are possible.

## Build strings whole

Never assemble a sentence from fragments around a variable (`"You have " + n + " new messages"`), because word order changes per language. Write one message with placeholders, and use the locale's plural rules rather than `n === 1 ? "" : "s"`. Many languages have more than two plural forms. Format numbers, dates and lists through `Intl`. Recipes are in patterns.md under [plurals](patterns.md#plurals-and-placeholders) and [locale formatting](patterns.md#locale-formatting).

## Verb-first buttons

A button label starts with a verb naming the action: "Send", "Save draft", "Delete project". Never "OK!" or "Let's go!".

An icon-only button's accessible name follows the same rule. It names the action, "Delete project", never the glyph, "Trash icon". The attribute that carries it is `better-accessibility`'s.

## Links describe their destination

Link text makes sense out of context, because screen-reader users navigate by a list of the page's links. Write "Read the billing docs", never "Click here".

A bare "Learn more" breaks down as soon as two appear on one page. Suffix each one: "Learn more about exports".

## One capitalization policy

Pick title case or sentence case per element type, then apply it to every instance of that type. Sentence case is the default where the project has no policy. "Save Changes" beside "Discard changes" reads as sloppiness.

## Settings describe the ON state

Label a toggle for what happens when it is on. "Send read receipts" lets users infer the off state; "Don't send read receipts" turns the toggle into a double negative.

Link straight to a referenced setting rather than describing the path to it: a "Notification settings" link, not "Go to Settings > Notifications > Email".

## Errors say how to fix, next to where it broke

An error is an instruction, and it belongs beside the field that failed:

| Bad | Good |
| --- | --- |
| That password is too short | Choose a password with at least 8 characters |
| Invalid date | Enter a date as DD/MM/YYYY |
| Oops! Something went wrong. | Unable to save. Check your connection and try again. |

No blame, no "oops" and no exclamation marks. Phrase hints positively, as in "Use at least 8 characters" rather than "Don't use fewer than 8". Show a known requirement as helper text before input, not only in the error. When the same error keeps firing, redesign the interaction so it cannot happen.

## Undo beats confirmation

Prefer undo when the action can be reversed and people perform it often, such as archiving, moving or deleting into a trash. Act at once and offer undo in the result: "Project archived. Undo".

Confirm before acting when the action cannot be reversed, affects other people or destroys many items at once. The confirmation repeats the consequence, so the dialog is answerable without reading the body:

- The title names the action and the object, "Delete 'Q3 report'?", never "Are you sure?".
- The body says what is lost and what cannot be recovered, with counts where they apply.
- The buttons are verb plus object, `Delete project` and `Cancel`, never `Yes` and `No`.
- For an account, a workspace or a repository, ask the person to type the object's name.

Templates are in [patterns.md](patterns.md#destructive-flows). The destructive button's distinct color is `better-colors`'.

## Empty states point forward

An empty state says what this place is and how to fill it, and offers one clear next action:

```html
<!-- Bad: a shrug -->
<p>No results.</p>

<!-- Good: orientation plus a next step -->
<p class="font-medium">No projects yet</p>
<p class="text-sm text-zinc-500">Projects keep your tasks and files together.</p>
<button class="mt-4">Create a project</button>
```

A filtered empty state names the query and offers an exit: "No results for 'quarterly'. Clear filters". Never park persistent information in an empty state. It disappears the moment content exists.

Loading, saving and success copy is in [patterns.md](patterns.md#status-copy).

## Placeholders show an example

A placeholder shows a realistic example in the format the field accepts, `name@example.com` or `DD/MM/YYYY`, never an instruction. Whether the field also needs a visible label is `better-accessibility`'s.

## Before you finish

| Detection pattern | Fix |
| --- | --- |
| `Click here`, a link reading `here` or two identical `Learn more` links | Name the destination |
| `Oops`, `Something went wrong` or `!` in an error string | Say what failed and the next step |
| `" + n + "`, a template literal holding a sentence fragment, or `=== 1 ? "" : "s"` | One message with a plural argument |
| `>OK<`, `>Yes<` or `>No<` on a dialog button | Verb plus object |
| `Are you sure` | Name the action and the object |
| `successfully` | Cut it: "Changes saved" |
| `Please` in a routine instruction | Cut it |
| A toggle label starting `Don't`, `Disable` or `Hide` | Describe the ON state |
| `aria-label="Trash"`, `"Close icon"` or `"X"` | Name the action: "Delete project", "Close dialog" |
| `toLocaleDateString()` with no locale, or a date built from `getMonth()` | `Intl.DateTimeFormat` with the user's locale |
| `Save Changes` beside `Discard changes` | One case per element type |

## Reporting

**Verification.** Check every label against the action it invokes, every error for a stated fix and every term against the inventory. Read locale files as well as components. Report strings supplied by the server or a CMS that you could not see as `Not verified`.

A standalone review takes its severity ladder and format from [reporting.md](reporting.md).
