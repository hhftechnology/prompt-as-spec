# Deterministic Gates Reference

Model self-inspection checklists catch obvious omissions, but empirical studies show code models still miss lint violations, legacy imports, and subtle bugs without external execution feedback. Pair every Layer 3 checklist with mechanical repo-level gates.

## 1. TypeScript / React: Enforce Import Bans with ESLint

Add `no-restricted-imports` to `eslint.config.mjs` so legacy or duplicate libraries fail CI immediately:

```javascript
// eslint.config.mjs - Mechanically enforce Layer 2 import constraints
export default [
  {
    rules: {
      // Block legacy or competing packages at lint time instead of relying on prompt compliance alone
      "no-restricted-imports": [
        "error",
        {
          paths: [
            {
              name: "framer-motion",
              message: "Import from 'motion/react' instead of legacy 'framer-motion'.",
            },
            {
              name: "@heroicons/react",
              message: "Use 'lucide-react' exclusively for UI icons.",
            },
          ],
          patterns: [
            {
              group: ["@radix-ui/react-*"],
              message: "Import from the unified 'radix-ui' package instead of individual @radix-ui/react-* packages.",
            },
          ],
        },
      ],
    },
  },
];
```

Recommended frontend gate command:

```bash
npx tsc --noEmit && npx eslint . && npm test -- --run
```

## 2. Discourse Theme / Component: Block Hardcoded Colors with Stylelint

Enforce Discourse semantic CSS custom properties so light and dark color schemes stay readable:

```json
{
  "rules": {
    "color-no-hex": true,
    "function-disallowed-list": ["rgb", "rgba", "hsl", "hsla"],
    "declaration-no-important": true
  }
}
```

Recommended theme verification commands:

```bash
npx eslint javascripts/ && npx stylelint "**/*.scss"
```

## 3. Discourse Ruby Plugin: RSpec and RuboCop Gate

Run both the Discourse RuboCop configuration and plugin RSpec suite before marking a task complete:

```bash
bundle exec rubocop plugins/<plugin-name>
LOAD_PLUGINS=1 bundle exec rspec plugins/<plugin-name>/spec
```

## 4. Go Backend: Race Detector, Vet, and golangci-lint

Minimal `.golangci.yml` focused on correctness and error hygiene:

```yaml
# .golangci.yml - Enforce error handling and static analysis hygiene
version: "2"
linters:
  enable:
    - errcheck
    - govet
    - ineffassign
    - staticcheck
    - unused
    - bodyclose
    - errorlint
```

Recommended Go gate command:

```bash
go vet ./... && go test -race -count=1 ./... && golangci-lint run
```

## 5. Astro 7 Corporate Website: Type Check and Production Build Gate

Verify TypeScript, Content Collection Zod schemas, and clean static build output:

```bash
npx astro check && npm run build
```

Optional quick check to catch accidental TODO stubs before delivery:

```bash
git grep -nE "TODO|FIXME|PLACEHOLDER" -- ':!*.md' || true
```
