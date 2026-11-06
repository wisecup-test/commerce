---
version: 2
name: cross-cutting-client-context-consumer-modules-resolve-dfad
description: "React Promise Unwrapping via use API at Context Boundaries: Client Context Consumer Modules Resolve Asynchronous"
metadata:
  actual:
    adr_id: dfad2433-f437-4b24-bbd5-51147ee24267
    status: accepted
    candidate_group_id: 20960f3d-e58a-4d3f-bbd1-4049364fd937
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
---

# React Promise Unwrapping via use API at Context Boundaries: Client Context Consumer Modules Resolve Asynchronous

Scope: applies to `components/cart/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ADR-001** MUST: Client context consumer modules MUST resolve asynchronous context promises using the React use API at component render boundaries rather than managing manual resolution state machines.

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