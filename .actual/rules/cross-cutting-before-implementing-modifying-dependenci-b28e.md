---
version: 2
name: cross-cutting-before-implementing-modifying-dependenci-b28e
description: "Adoption of next/link for Declarative Client-Side Navigation: Before Implementing Modifying Dependencies That Provide"
metadata:
  actual:
    adr_id: b28ef1a9-9b18-4928-9760-ceb47f0f2bb0
    status: accepted
    candidate_group_id: 2ab43c5c-4665-4c51-bd45-48eecd759009
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of next/link for Declarative Client-Side Navigation: Before Implementing Modifying Dependencies That Provide

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** MUST: Before implementing or modifying dependencies that provide routing primitives, the consumer MUST inspect the repository dependency resolution lock artifact to confirm the exact resolved version and validate that imported navigation APIs align with the documented interface for that version.

### Verify

```bash
# Discover and run the project static analysis and linting validation scripts
# Example: npm run lint

# Discover and run the project automated test suite
# Example: npm test
```

**Accept when:**
- All internal navigation elements across layout and presentation components resolve to next/link instances without unmanaged internal anchor elements.
- Static analysis and test suites execute with zero navigation-related lint errors or broken route reference failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>