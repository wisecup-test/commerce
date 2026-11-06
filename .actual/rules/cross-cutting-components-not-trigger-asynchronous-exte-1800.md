---
version: 2
name: cross-cutting-components-not-trigger-asynchronous-exte-1800
description: "React Client Component State and Effect Hooks Pattern: Components Not Trigger Asynchronous External Mutations"
metadata:
  actual:
    adr_id: 180025f8-7665-451d-9aff-68968071dfc5
    status: accepted
    candidate_group_id: 47fe226a-46d7-4ea3-8c60-35676ab361f8
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# React Client Component State and Effect Hooks Pattern: Components Not Trigger Asynchronous External Mutations

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-REACT-001** SHOULD_NOT: Components SHOULD NOT trigger asynchronous external mutations directly inside useEffect when an explicit user event handler can encapsulate the action.

### Verify

```bash
# Discover and run the test script in the project manifest
TEST_SCRIPT=$(node -p "require('./package.json').scripts.test || ''")
if [ -n "$TEST_SCRIPT" ]; then
  npm test
fi

# Discover and run the lint script in the project manifest
LINT_SCRIPT=$(node -p "require('./package.json').scripts.lint || ''")
if [ -n "$LINT_SCRIPT" ]; then
  npm run lint
fi

# Discover and run the build script in the project manifest
BUILD_SCRIPT=$(node -p "require('./package.json').scripts.build || ''")
if [ -n "$BUILD_SCRIPT" ]; then
  npm run build
fi
```

**Accept when:**
- Static analysis validates that all components consuming interactive hooks declare the client component directive.
- Unit and component tests verify that state transitions and effect hooks function correctly across user interactions.
- The build compilation step completes without type errors or server-client boundary violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>