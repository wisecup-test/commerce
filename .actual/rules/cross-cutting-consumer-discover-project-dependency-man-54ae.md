---
version: 2
name: cross-cutting-consumer-discover-project-dependency-man-54ae
description: "React Promise Unwrapping via use API at Context Boundaries: Consumer Discover Project Dependency Manifest Inspect"
metadata:
  actual:
    adr_id: 54aeac68-3abe-43aa-a222-e7adc4717bf8
    status: accepted
    candidate_group_id: 20960f3d-e58a-4d3f-bbd1-4049364fd937
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
---

# React Promise Unwrapping via use API at Context Boundaries: Consumer Discover Project Dependency Manifest Inspect

Scope: applies to `components/cart/**`.

These rules are ALWAYS ACTIVE for client components, context consumer hooks, and context providers encapsulating asynchronous resource promises for subtree consumption.

### Rules

- **R-ACT-001** MUST: The consumer MUST discover the project dependency manifest and inspect the repository lock artifact to determine the exact locked version of React before utilizing asynchronous context unwrapping APIs.

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