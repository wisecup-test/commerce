---
version: 2
name: cross-cutting-consumers-pass-custom-css-class-67bc
description: "Adoption of components/prose Module for Rich Text Typography: Consumers Pass Custom Css Class Names"
metadata:
  actual:
    adr_id: 67bc0c2e-77e5-4ff5-ab78-cdedf1177269
    status: accepted
    candidate_group_id: ef1ad579-dd4f-481b-8561-23724c669a3d
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
      - components/product/**
---

# Adoption of components/prose Module for Rich Text Typography: Consumers Pass Custom Css Class Names

Scope: applies to `app/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PROSE-001** MAY: Consumers MAY pass custom CSS class names to the prose component when specific layout constraints require contextual adjustments, provided base typographic rules remain intact.

### Verify

```bash
# Discover and execute the project test runner script
TEST_SCRIPT=$(npm pkg get scripts.test --json 2>/dev/null | tr -d '"' || node -e "console.log(require('./package.json').scripts.test)")
npm run ${TEST_SCRIPT:-test}

# Discover and execute the repository linter script
LINT_SCRIPT=$(npm pkg get scripts.lint --json 2>/dev/null | tr -d '"' || node -e "console.log(require('./package.json').scripts.lint || 'lint')")
npm run ${LINT_SCRIPT:-lint}

# Discover and execute the project build verification command
BUILD_SCRIPT=$(npm pkg get scripts.build --json 2>/dev/null | tr -d '"' || node -e "console.log(require('./package.json').scripts.build || 'build')")
npm run ${BUILD_SCRIPT:-build}
```

**Accept when:**
- The project test suite passes with all rich text rendering components mounting the prose module without errors.
- The project linter passes with zero violations regarding prohibited inline typography formatting in presentation templates.
- The project build verification command completes successfully with no type resolution or import errors.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>