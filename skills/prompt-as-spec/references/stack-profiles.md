# Stack Profiles (Layer 2 Reference)

Use these default Layer 2 tables and deterministic verification commands when authoring specifications or writing code. Combine rows only when a project genuinely spans multiple stacks, and prune unused rows to avoid rule bloat.

## 1. Discourse Ruby Plugin Development

| Domain           | Approved Choice                          | Conventions and Prohibited Patterns                                                                                                                   |
| :--------------- | :--------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Plugin Entry     | `plugin.rb` + `lib/` + `app/`            | Use official `Plugin::Instance` hooks, `DiscourseEvent`, `add_to_serializer`, and `add_to_class`. Do not monkey-patch core classes when a plugin API exists. |
| Client Code      | Ember Glimmer (`.gjs`)                   | Place UI extensions under `assets/javascripts/discourse/`. Use `api.renderInOutlet` and `.gjs` components. Do not add classic Ember components or raw `.hbs` template overrides. |
| Settings & Auth  | `config/settings.yml` + `Guardian`       | Guard plugin routes and jobs behind `SiteSetting.<plugin>_enabled`. Enforce permissions through `Guardian`; never skip authorization checks in controllers. |
| Database         | ActiveRecord migrations                  | Keep migrations reversible and indexed. Do not modify core Discourse tables directly when a plugin table or custom field suffices.                    |
| Localization     | `config/locales/server.en.yml`, `client.en.yml` | Keep all user-facing strings in locale YAML files in English. Do not hardcode UI copy inside Ruby controllers or `.gjs` templates.             |
| Verification     | RSpec + RuboCop                          | Run `bundle exec rspec plugins/<name>/spec` and `bundle exec rubocop plugins/<name>` with zero failures or offenses.                                  |

### Common Legacy-Drift Traps to Ban

- Raw `class_eval` / `prepend` overrides on core controllers when `Plugin::Instance` or `DiscourseEvent` hooks exist.
- Classic `.hbs` + `.js` component pairs instead of colocated Glimmer `.gjs` components.
- `api.modifyClass` on internal widgets or views where `api.renderInOutlet` or `api.registerValueTransformer` is supported.

## 2. Discourse Theme and Theme Component Development

| Domain           | Approved Choice                          | Conventions and Prohibited Patterns                                                                                                                   |
| :--------------- | :--------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Components       | Glimmer `.gjs` + Plugin Outlets          | Use `api.renderInOutlet` and `api.registerValueTransformer`. Do not override core Handlebars templates or rely on deprecated widget APIs.             |
| Styling & Tokens | SCSS + Discourse CSS custom properties   | Use semantic variables (`var(--primary)`, `var(--secondary)`, `var(--tertiary)`, `var(--highlight)`, `var(--primary-low)`). Zero hardcoded hex or RGB colors. |
| Configuration    | `settings.yml` + `locales/en.yml`        | Expose all configurable behavior via `settings.yml` with clear English descriptions. Do not hardcode domain names, category IDs, or group names.      |
| Structure        | Standard Discourse theme layout          | Keep `about.json`, `settings.yml`, `common/`, `desktop/`, `mobile/`, and `javascripts/discourse/` clean and minimal. Avoid `!important` cascades.     |
| Verification     | `discourse_theme` + ESLint + Stylelint   | Validate theme structure, lint JS and SCSS, and verify both light and dark Discourse color schemes plus mobile breakpoints.                           |

### Common Legacy-Drift Traps to Ban

- Hardcoded light-mode hex values (`#ffffff`, `#333333`) that make text unreadable on dark Discourse color schemes.
- Inline `<script type="text/discourse-plugin">` blocks in `head_tag.html` instead of proper modules under `javascripts/discourse/api-initializers/`.
- Deprecated widget-era hooks (`api.decorateWidget`, `api.reopenWidget`) on modern Glimmer-rendered surfaces.

## 3. Go Backend and TypeScript/React Frontend

| Domain           | Approved Choice                          | Conventions and Prohibited Patterns                                                                                                                   |
| :--------------- | :--------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Go HTTP & Core   | Go stdlib (`net/http`, `context`, `slog`)| Prefer standard library packages and explicit error wrapping (`fmt.Errorf("context: %w", err)`). Do not add heavy frameworks without escape-hatch justification. |
| Go Architecture  | Small interfaces + explicit DI           | Accept interfaces, return concrete structs. Propagate `context.Context` as the first parameter on all I/O functions. Never swallow errors with `_`.   |
| Go Verification  | `go test` + `go vet` + `golangci-lint`   | Run `go test -race ./...`, `go vet ./...`, and `golangci-lint run`. Zero race conditions or ignored errors.                                           |
| React + TS       | React + strict TypeScript                | Enable `"strict": true` in `tsconfig.json`. Zero `any` escape hatches without a justified inline comment.                                             |
| UI Components    | `shadcn/ui` + unified `radix-ui`         | Keep owned component source in `src/components/ui/`. Import from `radix-ui`, not scattered `@radix-ui/react-*` packages.                              |
| Animation        | `motion`                                 | Use `import { motion } from "motion/react"`. Do not import from legacy `framer-motion`.                                                               |
| Icons            | `lucide-react`                           | Use `lucide-react` exclusively. Do not mix Heroicons, Font Awesome, or Material Icons.                                                                |
| Styling & Tokens | Semantic CSS tokens / Tailwind tokens    | Reference semantic theme variables (`bg-background`, `text-foreground`, CSS vars). Zero raw hex colors in components.                                 |
| FE Verification  | `tsc --noEmit` + ESLint + Vitest/Jest    | Run `npx tsc --noEmit` and ESLint with `no-restricted-imports` enabled.                                                                               |

### Common Legacy-Drift Traps to Ban

- Importing `framer-motion` instead of `motion/react`.
- Installing individual `@radix-ui/react-dialog`, `@radix-ui/react-popover` packages instead of the unified `radix-ui` package (or mixing Radix and Base UI in the same repo).
- Mixing two state libraries (for example, adding Redux Toolkit when Zustand or React Context is already chosen).

## 4. Astro 7 Corporate Websites

| Domain           | Approved Choice                          | Conventions and Prohibited Patterns                                                                                                                   |
| :--------------- | :--------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Rendering        | Astro 7 static-first + Islands           | Ship zero client JavaScript by default. Only hydrate genuinely interactive islands (`client:visible` or `client:idle`); never wrap full pages in client React/JS. |
| Styling & Tokens | Semantic CSS custom properties           | Maintain one design token layer (colors, typography scale, spacing, radii) and one CSS approach. Do not mix competing CSS frameworks or UI kits.      |
| Content & Data   | Astro Content Collections + Zod          | Validate all Markdown/MDX/JSON content frontmatter with strict Zod schemas. Zero untyped content globbing.                                            |
| SEO & A11y       | Semantic HTML + structured metadata      | Enforce one `<h1>` per page, logical heading order, canonical URLs, Open Graph tags, `alt` attributes, and keyboard-accessible navigation.            |
| Assets           | Astro `astro:assets` (`<Image />`)       | Optimize local images with Astro's built-in image pipeline and explicit dimensions to prevent layout shift (CLS).                                     |
| Verification     | `astro check` + `astro build`            | Run `npx astro check` and `npm run build` cleanly; inspect bundle output to confirm static pages ship minimal or zero JS.                           |

### Common Legacy-Drift Traps to Ban

- Adding `client:load` everywhere when static Astro components or `client:visible` islands suffice.
- Pulling in a heavy React component library just for static marketing cards, grids, or footers.
- Hardcoding repeated header/footer markup instead of shared Astro layouts.
