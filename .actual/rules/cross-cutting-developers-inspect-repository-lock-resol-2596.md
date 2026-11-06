---
version: 2
name: cross-cutting-developers-inspect-repository-lock-resol-2596
description: "Adoption of Price Component Module for Standardized Monetary Presentation: Developers Inspect Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: 25962d53-662b-4bb3-af5d-e76924a63d99
    status: accepted
    candidate_group_id: da4c2516-68a9-4a63-8867-6fbbd796bc72
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - components/product/**
---

# Adoption of Price Component Module for Standardized Monetary Presentation: Developers Inspect Repository Lock Resolution Artifact

Scope: applies to `components/cart/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PRC-001** MUST: Developers MUST inspect the repository lock or resolution artifact to determine and verify the exact resolved versions of all runtime dependencies supporting currency formatting prior to modifying pricing display code.

### Verify

```bash
# Discover and run the project repository linter and static analysis suite to identify unauthorized inline price formatting.
# Discover and execute the automated component test suite to validate Price component rendering across varied currencies and quantities.
```

**Accept when:**
- All automated component tests and static analysis verification scripts complete with zero errors.
- No consumer interface components implement independent currency symbol concatenation or ad-hoc number formatting.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>