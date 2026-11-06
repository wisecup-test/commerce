---
version: 2
name: cross-cutting-consumer-components-pass-custom-visual-258a
description: "Adoption of Price Component Module for Standardized Monetary Presentation: Consumer Components Pass Custom Visual Container"
metadata:
  actual:
    adr_id: 258a0fae-76d0-4409-8620-c727d8752f9e
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

# Adoption of Price Component Module for Standardized Monetary Presentation: Consumer Components Pass Custom Visual Container

Scope: applies to `components/cart/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PRICE-001** MAY: Consumer components MAY pass custom visual container styling properties to the shared Price component when surface-specific typography or spacing adjustments are required.

### Verify

```bash
# Discover and run the project repository linter and static analysis suite to identify unauthorized inline price formatting.
# Discover and execute the automated component test suite to validate Price component rendering across varied currencies and quantities.
```

**Accept when:**
- All automated component tests and static analysis verification scripts complete with zero errors.
- No consumer interface components implement independent currency symbol concatenation or ad-hoc number formatting.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>