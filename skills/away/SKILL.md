---
name: away
description: Works through a project while you're away, finding improvements and new ideas, building the strongest few on a local branch and leaving a report for when you're back.
disable-model-invocation: true
---

# Away

This skill spends the time you're gone making one project better. It gathers evidence, generates ideas widely, builds the strongest few on a local branch and leaves a report you can read in two minutes.

Depth beats speed here. The person is gone for hours, so when the first batch is built and verified, take the next idea from the list instead of stopping.

Reviewing an interface is `better-interface`, reviewing a diff is `interface-review`, exploring alternatives for one component is `variant` and stress testing one is `break`.

## Better for the people who use it

Every idea answers one question: would someone using this product genuinely find it better? Name that person and the moment it helps them, such as a reader landing on a dead link or a visitor hunting for a post. An idea that only makes the code tidier or the agent look busy fails the test.

Rank what passes by its evidence of need. Real traffic landing on a 404 counts, and so does an error in production. So does a promise the product's copy doesn't keep, or a fix the person already shipped in a sibling project. An idea with no evidence can still be built, but it loses every tie and its diff stays small.

## Sometimes out of the box

Most nights the strongest ideas are fixes and polish. Some nights one idea should be unexpected. It might be a feature the product's own content makes possible, a new way to browse it or a tool that changes how the person works. Build one when it passes the test above and fits in a small diff. Otherwise it goes in the report with the reasoning that makes it worth trying.

A list with only safe fixes means the generating step was cut short.

## Match the repository

A change should read as if the person wrote it. Before building anything, find how the repository already solves the nearest problem and copy that:

- Reuse its components, hooks and utilities. A new primitive needs a reason the existing ones can't cover.
- Use its design tokens for color, spacing, radius, type and motion. Take values from the nearest existing surface and invent none.
- Follow its file layout, naming, import style and the rules in its agent docs, including when to write comments.
- Write interface copy in the voice the product already uses, at its length and with its punctuation.
- Keep its architecture. A new state library, styling approach or data layer is an idea for the report, never something to build unattended.

When a sibling project solved the same problem, port that solution and restyle it with this repository's tokens.

## Stay reversible

The person comes back to choices, not to cleanup:

- Work on a new branch named `away/<date>` from the default branch. When the working tree holds uncommitted work, build in a separate `git worktree` and leave that work exactly as it was.
- Commit locally, one change per commit, in the repository's own commit style. Push, open pull requests, merge, deploy, post and send messages only when this request asked for it.
- Treat connected services as read-only. Read analytics, error trackers and deploy logs, and change no settings, data or integrations.
- Leave the person's own writing in their voice. Draft new copy, changelog entries or posts in the report rather than editing published prose.
- Add a dependency only when a sibling project already uses it for the same job, and name it in the report.
- Snapshot any file a test run writes to, and restore it from the snapshot afterwards.

## 1. Learn the project

Read `AGENTS.md` or `CLAUDE.md`, the README, the recent git log, open branches and pull requests and any saved memory about the person. Note the conventions they enforce, such as commit format, comment rules and component patterns. Done when you can state the project's purpose, its stack and the conventions a reviewer would reject a change for breaking.

## 2. Gather evidence

Read every source that is connected. That means web analytics for top pages, 404s and referrers, error trackers and runtime logs, CI results, open issues and `TODO` comments. It also means the product's copy and docs, checked against what the product actually does, and the console on its busiest pages.

Then read the person's sibling projects for something they already solved there, such as a search dialog, a component or a workflow. Done when each source has been read or set aside with a reason.

## 3. Generate widely

Write at least fifteen ideas before choosing any. Cover fixes, design polish, features, content, performance, accessibility and the person's own tooling, such as a skill or a script. Include at least three that change what the product does, not only how it looks. Give each idea the person it helps, its evidence, its rough size and what it risks. Cut any idea whose person you can't name. Done when every area has an idea and at least one would surprise the person.

## 4. Build the strongest few

Choose the ideas with the best evidence that each land as one small commit. Fix real user pain first, then build features, then polish. Build each as **Match the repository** describes, and run the project's check command before committing. Done when each chosen idea is committed or dropped with the reason it was dropped.

## 5. See each change working

Exercise every change where a person would meet it. Use the browser for interface work, `curl` for routes and redirects and the real endpoint for anything AI-backed. Where a change is visual, look at it in light and dark themes and at phone width. Done when each change has been seen working or is marked unverified.

## 6. Leave the report

End with one message the person reads first, in this order:

1. **Where things are.** The branch, the commit count, any dev server URL and anything left running.
2. **What was built.** One entry per change, saying what a user now sees and the evidence that motivated it.
3. **What was verified.** Seen working, kept apart from checked only by types or not checked at all.
4. **Decisions for you.** Choices made on the person's behalf that they may want to reverse.
5. **Ideas not built.** The rest of the list, wild ones included, each with its evidence and size.

Drafts from **Stay reversible** go with the ideas, ready to paste.

## Before you finish

| Pattern | Fix |
| --- | --- |
| A commit subject needs "and" | Split it into two commits |
| A change is verified only by a type check | Exercise it in the running app or mark it unverified |
| The report lists commit subjects | Say what a user sees now, and why it was built |
| A diff touches files the person had uncommitted | Move the work to a separate worktree and restore theirs |
| A comment explains what the code does | Delete it unless the code would look wrong without it |
| The ideas list holds only fixes | Go back to **3. Generate widely** for the wild ones |
| A built idea helps no one you can name | Revert it and move it to the report |
| A new color, spacing value or component appears in the diff | Replace it with the repository's nearest existing one |
| Work stopped with time left and ideas unbuilt | Build the next evidenced idea |
