---
description: Interface review of a change rather than a screen: uncommitted work, the current branch, or a pull request. Pass an optional mode and target, e.g. "quick pr 482".
---

Invoke the skill tool to load the skill named `interface-review`, then run the change review exactly as that skill defines it, resolving the change scope, expanding it to affected surfaces, reading both sides of the diff, and classifying every finding. The invocation is: $ARGUMENTS. When no target is supplied, resolve it from version control as the skill instructs. Never mutate the working tree. When the review is complete, hand it to `better-interface` as the skill requires.