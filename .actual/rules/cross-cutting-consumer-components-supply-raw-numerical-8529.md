---
version: 2
name: cross-cutting-consumer-components-supply-raw-numerical-8529
description: "Adoption of Price Component Module for Standardized Monetary Presentation: Consumer Components Supply Raw Numerical Amounts"
metadata:
  actual:
    adr_id: 85295327-70e0-43f6-a10c-147d3106ec99
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

# Adoption of Price Component Module for Standardized Monetary Presentation: Consumer Components Supply Raw Numerical Amounts

Scope: applies to `components/cart/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PRC-001** SHOULD: Consumer components SHOULD supply raw numerical amounts and explicit currency codes to the shared Price component interface to ensure deterministic rendering across different locales.

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