---
version: 2
name: cross-cutting-components-consuming-promises-via-react-519f
description: "React Promise Unwrapping via use API at Context Boundaries: Components Consuming Promises Via React Use"
metadata:
  actual:
    adr_id: 519f3cd0-9703-452d-862d-a4bdc9da4321
    status: accepted
    candidate_group_id: 20960f3d-e58a-4d3f-bbd1-4049364fd937
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
---

# React Promise Unwrapping via use API at Context Boundaries: Components Consuming Promises Via React Use

Scope: applies to `components/cart/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ACT-001** MUST_NOT: Components consuming promises via the React use API MUST NOT catch resolution rejections within the render cycle using inline try-catch blocks and MUST propagate rejections to enclosing error boundaries.

### Verify

```bash
# Discover the project test runner from the repository manifest and execute component test suites verifying context promise resolution.
# Discover the static analysis verification command from the repository configuration and validate hook usage rules across context consumer modules.
```

**Accept when:**
- All context consumer components unwrapping promises compile and pass static verification without hook rule violations.
- Component tests verify successful data unwrapping and proper fallback handling during pending promise states.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>