# FlowMVI Docs Anti-Pattern Checklist

Validation tool for the `flowmvi-docs` skill. Scan every draft — each hit is a must-fix.

## Checklist

- [ ] Marketing or promotional language ("powerful", "seamless", "What You Gain", "best-in-class")
- [ ] Self-deprecating language ("FlowMVI does not provide...", "unfortunately", "caveat")
- [ ] Hedging voice ("you might consider", "one approach could be", "optionally")
- [ ] Code examples with imports (unless resolving ambiguity)
- [ ] Inline comments restating the obvious (`// create the store`, `// Contract`, `// Container using lambda intents`)
- [ ] Type references instead of function references (`MVIAction` instead of `action()`, `MVIState` instead of `updateState {}`)
- [ ] Duplicated code blocks across sections or pages
- [ ] Bare admonitions without titles (`:::tip` instead of `:::tip[Title]`)
- [ ] `init {}` used for flow collection (should be `whileSubscribed {}`)
- [ ] `whileSubscribed` framed as optional or "layer-in-later" (it is mandatory from day one)
- [ ] Testing sections that reproduce MVVM comparison boilerplate
- [ ] Internal test utilities in public-facing examples (`testStore()` etc.)
- [ ] Undeclared variables in code snippets (e.g. `snackbarHostState` without `remember {}`)
- [ ] Missing null guards on nullable values passed where non-null is expected
- [ ] "on Android" for features that work on all KMP targets
- [ ] Migration examples that change the reader's DI, navigation, or lifecycle patterns
- [ ] Multiple tabs or "alternative implementations" showing the same feature in different styles
- [ ] Off-topic sections bolted on (heading promises X, content covers Y)
- [ ] Dangling cross-references ("See the testing guide..." with no link)
- [ ] `collectMetrics()` or advanced plugins listed in introductory / migration docs

## Review Workflow

When validating an AI-generated or drafted page:

1. Read the page end-to-end once. Note the heading, intended audience, and scope.
2. Walk the checklist. For each hit, quote the exact phrase, name the rule, and suggest the concrete replacement.
3. Mentally compile every code example. Flag any undeclared variables, unresolved types, or missing null guards. A snippet that will not compile teaches nothing.
4. Verify every admonition carries an explicit title (`:::tip[Title]`).
5. Confirm no section duplicates content from another page. If it does, link instead.
6. Confirm heading and content scope match. A section titled "Reactive Data" should not contain unrelated sealed-class intent examples.
7. Verify naming conventions hold in all examples (Intents past-tense, Actions present-verb, States gerund).

## Output Format for Reviews

Report each violation as:

```
<rule name>  — "<exact quoted phrase>"
  Why: <one line>
  Fix: <concrete replacement>
```

Group by severity: crash/compile blockers first, then style violations, then polish.
