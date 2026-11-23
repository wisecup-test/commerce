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

These rules are ALWAYS ACTIVE for:
- User interface presentation components displaying product pricing, line item costs, or order subtotals
- Interactive shopping surfaces including product details and shopping cart interfaces

### Rules

- **R-LM-001** MAY: Consumer components MAY pass custom visual container styling properties to the shared Price component when surface-specific typography or spacing adjustments are required.

### Verify

- Discover and run the project repository linter and static analysis suite to identify unauthorized inline price formatting.
- Discover and execute the automated component test suite to validate Price component rendering across varied currencies and quantities.

**Accept when:**

- All automated component tests and static analysis verification scripts complete with zero errors.
- No consumer interface components implement independent currency symbol concatenation or ad-hoc number formatting.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis checks in the continuous integration pipeline
- Verified by: Peer code review verification on all pull requests touching user interface presentation modules
- Violation handling: Pull requests containing inline price formatting or bypassing the shared component are blocked from merging until refactored.
- Exception process: Submit an architectural review request detailing third-party embed requirements or unresolvable layout constraints to the Frontend Architecture Lead for formal evaluation.
</enforcement>
