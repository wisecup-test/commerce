---
version: 2
name: cross-cutting-consumer-components-not-implement-custom-34fa
description: "Adoption of Price Component Module for Standardized Monetary Presentation: Consumer Components Not Implement Custom Currency"
metadata:
  actual:
    adr_id: 34fa6887-4493-4baa-be42-650f251a968c
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

# Adoption of Price Component Module for Standardized Monetary Presentation: Consumer Components Not Implement Custom Currency

Scope: applies to `components/cart/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PRC-001** MUST_NOT: Consumer components MUST NOT implement custom currency formatting routines, manual string concatenations of currency symbols, or localized number parsing outside of the designated shared Price module.

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