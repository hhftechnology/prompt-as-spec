---
name: prompt-as-spec
description: Turn rough feature ideas into strict three-layer engineering specification prompts and enforce architectural guardrails when building or reviewing code across Discourse Ruby plugins, Discourse themes and components, Go backends, TypeScript/React frontends, and Astro 7 corporate websites. Use when the user asks to write a coding prompt, spec out a feature, scaffold a project, prevent dependency bloat or legacy API drift, or enforce strict stack conventions and acceptance checklists.
---
# Prompt as Spec

Turn vague feature descriptions into compact, verifiable engineering specifications and apply those same guardrails when writing or reviewing code. Negative constraints narrow the model's search space more effectively than aesthetic prose, while a controlled escape hatch and deterministic CLI checks prevent both dependency sprawl and over-constrained output.

## When to Use

- The user wants a ready-to-use specification prompt for a coding agent or new session.
- The user is scaffolding, building, or reviewing code in Discourse (Ruby plugins or themes/components), Go + TypeScript/React, or Astro 7.
- The user wants to stop an AI coding workflow from mixing libraries, importing deprecated packages, hardcoding colors, or leaving placeholder stubs.

## Bundled Resources

Load only the supporting files needed for the current task:

- `references/stack-profiles.md` - Detailed Layer 2 stack tables, legacy-drift bans, and verification commands for Discourse Ruby plugins, Discourse themes/components, Go + TypeScript/React, and Astro 7.
- `references/deterministic-gates.md` - Ready-to-use lint, typecheck, and CI configuration snippets (`eslint.config.mjs`, `.stylelintrc.json`, `.golangci.yml`, RuboCop/RSpec, and Astro checks) that enforce Layer 2 and Layer 3 rules mechanically.
- `references/research-rationale.md` - Empirical backing (2024 to 2026 studies) explaining why negative constraints, package pinning, escape hatches, and deterministic gates work and where over-constraining degrades accuracy.
- `assets/system-spec-template.md` - Copy-pasteable three-layer System Specification template with the controlled dependency escape hatch.
- `assets/task-prompt-template.md` - Copy-pasteable separate User Task Prompt template for product requirements.

## Core Principles

- Favor binary negative constraints ("do not import X") over vague positive adjectives ("clean, modern, dynamic").
- Keep constraints scoped to architectural boundaries, package hygiene, and accessibility. Omit theatrical personas and artificial urgency framing.
- Always include a controlled escape hatch for new dependencies so the model justifies genuine additions instead of hand-rolling fragile custom implementations.
- Pair every acceptance checklist item with a deterministic local check (linter, typechecker, test runner, or build command) rather than relying on model self-inspection alone.
- Keep all code, comments, prompts, and checklists in English.

## Workflow

### Step 1: Identify Mode and Target Stack

Determine whether the user wants:

1. **Spec Prompt Output:** A two-part deliverable (System Specification + separate Task Prompt) ready to paste into a coding agent. Load `assets/system-spec-template.md` and `assets/task-prompt-template.md`.
2. **Direct Implementation or Review:** Enforcing the three-layer specification while writing, refactoring, or auditing code in the current session.

Load `references/stack-profiles.md` and select only the stack rows relevant to the project. If a project spans multiple stacks (for example, a Go API paired with a React UI), combine only the matching rows.

### Step 2: Build the Three-Layer Specification

Structure every specification in three distinct layers so rules stay scannable and non-contradictory:

1. **Layer 1 (Hard Principles):** Write 4 to 6 short, non-negotiable architectural rules (single UI/styling system, zero duplicate dependencies, platform/stdlib APIs first, tokenized styling only, zero TODO stubs or fake handlers).
2. **Layer 2 (Technology Stack and Conventions Table):** Pin exact package names, import paths, and banned legacy patterns from `references/stack-profiles.md`. Prune any rows irrelevant to the task.
3. **Layer 3 (Acceptance Checklist and Deterministic Gates):** Write binary pass/fail items and attach the exact CLI verification commands and lint configs from `references/deterministic-gates.md`.

### Step 3: Add the Controlled Escape Hatch

Append the structured exception rule so strict dependency limits do not force bad hand-rolled code:

```text
If a new dependency is genuinely required, document three items in your delivery notes before adding it:
1. Why the package is needed.
2. What capability or manual implementation it replaces.
3. Why existing approved dependencies or standard APIs cannot cover the requirement.
```

### Step 4: Separate "How to Build" from "What to Build"

Keep Layers 1 through 3 plus the escape hatch in the **System Specification** (or repo rule file such as `AGENTS.md` or `CLAUDE.md`). Put product scope, pages, modules, and functional requirements in a **separate Task Prompt** so architectural rules stay reusable across tasks.

## Gotchas

- **Over-constraining kills accuracy:** Keep Layer 1 to 4 to 6 high-impact architectural rules. Adding dozens of stylistic micro-rules or roleplay personas causes frontier models to become hyper-literal or drop requirements.
- **System prompts can leak under conflict:** If a task prompt contradicts a Layer 1 rule, models may follow the task prompt. Mirror critical import and dependency bans in ESLint, Stylelint, RuboCop, `golangci-lint`, or CI.
- **Unverified checkboxes create false confidence:** Never mark a checklist item complete based on visual reading alone when a CLI check can prove it.
