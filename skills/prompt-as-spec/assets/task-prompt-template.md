# Task Prompt Template

Send this as a separate user message after the System Specification is loaded. Keep architectural rules out of this message so it stays focused strictly on what to build.

```markdown
Product / Feature: <One-sentence summary of what is being built>
Target Stack: <Discourse Ruby Plugin | Discourse Theme Component | Go + React/TS | Astro 7>

Core Pages / Endpoints / Outlets:
- <Page, route, API endpoint, or Discourse plugin outlet>
- <Page, route, API endpoint, or Discourse plugin outlet>

Key Modules and Behaviors:
- <Concrete module with required interactions, validation, or state>
- <Concrete module with required interactions, validation, or state>

Required Non-Functional Constraints:
- <Performance, offline, accessibility, SEO, or permission requirements>

Out of Scope:
- <Explicitly list what NOT to build in this pass>
```
