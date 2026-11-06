---
version: 2
name: cross-cutting-user-interface-modules-that-render-dca8
description: "Adoption of Price Component Module for Standardized Monetary Presentation: User Interface Modules That Render Product"
metadata:
  actual:
    adr_id: dca8acc7-e269-48cc-b8f1-448fd0599ff8
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

# Adoption of Price Component Module for Standardized Monetary Presentation: User Interface Modules That Render Product

Scope: applies to `components/cart/**`, `components/product/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-PRC-001** MUST: User interface modules that render product costs, subtotal figures, or currency values MUST import and delegate rendering to the shared Price component module rather than formatting monetary strings inline.

### Verify

```bash
# Discover and run the project repository linter and static analysis suite to identify unauthorized inline price formatting.
# Discover and execute the automated component test suite to validate Price component rendering across varied currencies and quantities.
```

**Accept when:**
- All automated component tests and static analysis verification scripts complete with zero errors.
- No consumer interface components implement independent currency symbol concatenation or ad-hoc number formatting.

<enforcement>
Claude Code MUST NOT skip or defer verification. All pull requests containing inline price formatting or bypassing the shared component are blocked from merging until refactored.
</enforcement>