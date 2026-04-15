---
name: flowmvi-docs
description: Use when writing, editing, or reviewing documentation pages for the FlowMVI project (docs/docs/** guides, migration pages, integration docs, API references, plugin catalog), or when validating AI-generated FlowMVI docs against Nek-12's doc philosophy.
---

# FlowMVI Documentation Style

## Overview

FlowMVI docs are prescriptive technical reference. No marketing. Point to functions, not types. One authoritative example per concept across the whole site. Validation tool: `references/checklist.md`.

## When to Use

- Writing a new page under `docs/docs/**`
- Editing an existing doc page
- Reviewing AI-generated or human-drafted doc content before commit or PR

Not for KDoc, CHANGELOG, commit messages, or README.

## Core Principles

1. **Technical reference, not marketing.** Drop "powerful", "seamless", "What You Gain".
2. **Prescriptive voice.** "Use `X`", "you must" — not "you might consider".
3. **No self-deprecation.** State the alternative instead of "FlowMVI does not provide X".
4. **Point to functions, not types.** `action()`, `subscribe()`, `updateState {}`, `reduce {}`, `whileSubscribed {}`, `intent()` — not `MVIAction`, `MVIState`. Types on first introduction only, then use function names.
5. **No duplication.** Link, don't copy. One authoritative code block per concept across the whole site.
6. **Code must compile mentally.** Every variable declared, every type resolvable, no `// ...` placeholders.
7. **Minimize migration friction.** Reader's ViewModels, DI, navigation, and lifecycle patterns do not change in migration examples — only FlowMVI-specific parts differ.

## Structure: Explanation → Code → Deep Dive

1. One or two sentences on what the feature does. No code yet.
2. Minimal, correct, complete working example.
3. Edge cases, gotchas, configuration — after the example.

Use `<details>` blocks only for supplementary deep dives. Essential content stays uncollapsed.

## Admonitions Require Explicit Titles

Never bare `:::tip` / `:::warning` / `:::danger` / `:::info`.

```markdown
:::warning[Plugin order matters]
Changing plugin order may completely change how your store works.
:::
```

| Type | Use for |
|------|---------|
| `:::danger[Title]` | Architectural violations, crash-causing mistakes |
| `:::warning[Title]` | Important gotchas, performance concerns |
| `:::tip[Title]` | Shortcuts, optimizations, convenience features |
| `:::info[Title]` | Context, historical notes, "why" explanations |

## Code Examples

- No imports unless resolving ambiguity.
- No useless comments (`// create the store`, `// Contract`). Comment only non-obvious behavior.
- Sparse type annotations: explicit on first introduction, omit once the pattern is familiar.
- Use public `store()` / `container()` builders. No internal test utilities (`testStore()`).
- DSL in prose uses braces: `reduce { }`, `recover { }`, `whileSubscribed { }`.
- Plugin install phrasing: "Install with `pluginName { }`".
- Before/After: inline ❌/✅ with names like `broken`/`working`. The "after" keeps existing ViewModel/DI/navigation — only FlowMVI parts change.

## Migration Docs — House Rules

From PR #220 review feedback. Non-negotiable.

- **`whileSubscribed` is mandatory from day one** for reactive data. Never show `init {}` for flow collection — it does not cancel when subscribers disappear. #1 MVVM-migrant footgun.
- **MVVM+ (lambda intents) is the default migration path.** Sealed-class intents are an upgrade, not the starting point.
- **One authoritative example per feature.** No tabs, no "alternative implementation of the same feature".
- **Events:** show Google's events-as-state (nullable fields, `consumeX()` callbacks, `LaunchedEffect` key issues) as the "before"; `action()` as the clean replacement.
- **Plugin config:** logging and shared config live in DI setup once, not scattered per-store. Do not list `collectMetrics()` in introductory or migration docs.
- **Testing:** one compact example using the public `store()` builder, then link to the testing guide. Do not reproduce MVVM test boilerplate.

## Platform Content

KMP-first. Never say "on Android" for a feature that works everywhere. Platform-specific content lives in `integrations/android.md`, `integrations/compose.md`, etc. If code is identical across platforms, say so once in a `:::tip[Title]` — do not copy snippets into `<details>` blocks per platform.

## Naming Conventions

- **Intents:** `<PastTenseAction><Target>` — `ClickedCounter`, not `CounterClick`
- **Actions:** `<PresentTenseVerb>` — `ShowConfirmationPopup`, not `ConfirmationShown`
- **States:** `<PresentTenseGerund>` — `EditingGame`, not `Edit` or `GameEdit`

## Validation Mode

To review an AI-generated or drafted page, load `references/checklist.md` and follow the workflow there. Quote violations verbatim, name the rule, suggest concrete replacements.

## Cross-References

- API questions while drafting code examples: use the `flowmvi` skill (sibling).
- Real doc exemplars to pattern-match: `docs/docs/migration/mvvm.md`, `docs/docs/plugins/delegates.md`, `docs/docs/plugins/custom.md`.
