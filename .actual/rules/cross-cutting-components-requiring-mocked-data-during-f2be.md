---
version: 2
name: cross-cutting-components-requiring-mocked-data-during-f2be
description: "Adoption of lib/shopify as Core E-Commerce Integration Module: Components Requiring Mocked Data During Isolated"
metadata:
  actual:
    adr_id: f2be54f6-5464-4272-a24b-283186791701
    status: accepted
    candidate_group_id: 4351d544-a887-4734-b4b3-6c3c9ad9ed15
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/layout/**
---

# Adoption of lib/shopify as Core E-Commerce Integration Module: Components Requiring Mocked Data During Isolated

Scope: applies to `components/layout/**`.

These rules are ALWAYS ACTIVE for:
- User interface components and structural layout elements requiring access to store resources or operational domain data.
- Data fetching and domain mutation operations interacting with the external e-commerce platform.

### Rules

- **R-LM-001** MAY: Components requiring mocked data during isolated verification MAY substitute the lib/shopify module exports with compliant test doubles.

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
