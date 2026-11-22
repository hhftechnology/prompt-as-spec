# System Specification Template

Copy and customize this block into your system prompt, `AGENTS.md`, or `CLAUDE.md`. Keep it concise and prune any stack rows that do not apply to the repository.

```markdown
## Layer 1: Hard Principles

1. Single design and component system: Use one approved UI/styling system across the project. Do not introduce a second component or icon library.
2. Zero duplicate dependencies: Allow only one library per capability.
3. Prefer platform and standard APIs first: Reuse official framework hooks, standard library packages, or existing repo utilities before adding external packages.
4. Tokenized styling only: Zero hardcoded hex/RGB colors; use semantic CSS custom properties or design tokens that work in both light and dark modes.
5. Complete implementations only: Zero TODO stubs, omitted code blocks, or fake mock handlers in delivered code.
6. English only: Write all code, identifiers, comments, locale strings, and delivery notes in English.

## Layer 2: Technology Stack and Conventions

| Domain       | Approved Choice              | Conventions and Prohibited Patterns                                        |
| :----------- | :--------------------------- | :------------------------------------------------------------------------- |
| <Domain 1>   | <Package / Framework>        | <Exact import path, directory location, and banned legacy alternatives>    |
| <Domain 2>   | <Package / Framework>        | <Exact import path, directory location, and banned legacy alternatives>    |
| Verification | <Lint / Typecheck / Test>    | <Exact CLI commands that must exit 0 before completion>                    |

## Layer 3: Acceptance Checklist

Before finishing, verify every item and run the deterministic gates listed above:

- [ ] Every capability is served by at most one dependency (no duplicate libraries)
- [ ] Zero banned or legacy imports remain in the diff
- [ ] Zero hardcoded colors; light and dark modes maintain readable contrast
- [ ] All interactive controls are keyboard-reachable with visible focus indicators
- [ ] Zero TODOs, omitted code blocks, or placeholder implementations
- [ ] All deterministic CLI checks (lint, typecheck, test, build) pass cleanly

## Controlled Dependency Escape Hatch

If a new dependency is genuinely required, document three items in your delivery notes before adding it:
1. Why the package is needed.
2. What capability or manual implementation it replaces.
3. Why existing approved dependencies or standard APIs cannot cover the requirement.
```
