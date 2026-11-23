---
version: 2
name: cross-cutting-components-consuming-commerce-data-impor-d865
description: "Adoption of lib/shopify as Core E-Commerce Integration Module: Components Consuming Commerce Data Import Shared"
metadata:
  actual:
    adr_id: d865e829-fa6b-4a83-b527-d386ba74aa6e
    status: accepted
    candidate_group_id: 4351d544-a887-4734-b4b3-6c3c9ad9ed15
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/layout/**
---

# Adoption of lib/shopify as Core E-Commerce Integration Module: Components Consuming Commerce Data Import Shared

Scope: applies to `components/layout/**`.

These rules are ALWAYS ACTIVE for:
- User interface components and structural layout elements requiring access to store resources or operational domain data.
- Data fetching and domain mutation operations interacting with the external e-commerce platform.

### Rules

- **R-LM-001** MUST: Components consuming e-commerce data MUST import shared entity contracts from lib/shopify/types rather than defining local duplicate type definitions.

### Verify

- Discover and execute the repository type checking script to verify that all layout components consume lib/shopify and lib/shopify/types without compilation errors.
- Discover and run the architectural linter configured in the project to verify that no direct external e-commerce API calls exist outside of lib/shopify.

**Accept when:**

- Static type analysis passes with zero type errors across all layout component imports.
- Boundary linting confirms all e-commerce operations originate exclusively from lib/shopify.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated continuous integration checks executing type verification and import boundary rules.
- Verified by: Peer code review for pull requests modifying layout components or e-commerce integration code.
- Violation handling: Pull requests introducing direct external API calls or duplicate domain types are blocked until refactored to consume lib/shopify.
- Exception process: Architecture review and documented justification approved by the engineering team prior to bypassing the module.
</enforcement>
