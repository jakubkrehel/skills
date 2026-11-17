---
name: break
description: Renders a component you choose under every scenario that can reach it on a temporary page and stress tests it.
disable-model-invocation: true
---

# Break

This skill takes one component and renders it on a harness page under every scenario that can actually reach it. That page is the deliverable: a visual report the user scrolls through, every scenario on one page, with the breaks marked.

It observes rather than judges. A finding here is something that visibly broke on the page, named in the vocabulary of the domain skill that owns the fix. Reviewing code against a standard is `interface-review` and `better-interface`; exploring design alternatives is `variant`; working through a component's product states one at a time is `state-machine`.

Where `variant` insists on the real page, this skill isolates on purpose. You are not judging how the component looks in context. You are checking whether it defends itself when the content is worst-case.

The whole run is build, look once, report: minutes, not a session. Nothing in it justifies instrumentation, browser debugging or a second pass.

## 1. Scope one component

One component per run. "The settings page" is not a component; the profile form's text input is. Where the request spans several, list the candidates and ask which one to test.

Restate what the component is in one sentence: what it accepts, what it renders and where it will live.

## 2. Infer the scenarios from the component

Stress only what varies. A scenario earns a slot when the component accepts something that can take that shape in production. So read the component first: its props, its slots, its states and the data it renders.

[scenarios.md](scenarios.md) holds the axes, the values on each and the cue that says whether an axis applies. Walk it against the component and keep only the axes whose cue matches. A text input gets content length and states, never item quantity. A static icon button with a fixed label gets container and environment, never long text.

Write the kept scenarios down before building, one line each. Then say which axes you dropped and why, in one line, so a wrong inference is cheap to catch.

## 3. Build the harness page

The harness page renders the component once per scenario in a single column, with a short text label above each instance.

A scratch route inside the app holds the real component, imported from the project and untouched. The route inherits the app's layout, fonts, global styles and providers for free.

Where the framework splits server from client components, the page is client code, `"use client"` in Next. Otherwise fixture data can silently vanish crossing that boundary, and the page renders empty.

Labels, container widths and fixture props are everything the page adds. No fonts or styles of its own, no simulated themes or token swaps, no probes. A component observed under any of those is a different component.

Feed scenarios as props and fixture data. The harness never imports production state, never wires to live data and production never imports from the harness.

## 4. Look once

Load the page once in a browser already at hand, skim every scenario top to bottom and note what visibly broke. "Text escapes the field's right edge", never "spacing feels tight". A predicted failure is not a finding.

With no browser at hand, say so and hand the URL over for the user to check. Launching, window-wrangling or debugging a browser is out of budget. When you skipped the look, the report is the URL and the scenario list, with no findings table and no page notes.

Then mark each break you did see on the page, a one-line note under that scenario's label, in a single edit. The page has to read as the report on its own.

## 5. Report what broke and stop

Report findings as a table, broken scenarios first:

| Scenario | Observed | Owner |
| --- | --- | --- |
| One unbreakable 60-character string | Overflows the card, no wrap and no truncation | `better-typography` |
| Zero items | Blank region with no message | `better-writing` |

This skill owns no domain rules and issues no verdict.

"Everything survived" is a complete and useful report. Say which scenarios are on the page and where it is running, so the user can see every one themselves. End there rather than padding the result with preferences.

Do not fix anything unasked. On a request to fix, follow the owner skill's rules, then re-render the failing scenarios to confirm.

## 6. Leave the page up, delete it on request

The page is half the report, so it outlives the findings table. Leave it running. Delete it and its fixtures only when the user says they are done with it. Then search for the route and fixture names, and check the diff leaves nothing of the harness behind.

## Before you finish

| Mistake | Fix |
| --- | --- |
| A scenario missing the content it was fed | The harness is broken, not the component; make the page client code and reload once |
| The viewport resized scenario by scenario | Widths are fixed containers on the page; one load shows them all |
| A break in the table but unmarked on the page | Note it under the scenario's label; the page reads as the report on its own |
