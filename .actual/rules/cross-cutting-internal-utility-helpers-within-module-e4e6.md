---
version: 2
name: cross-cutting-internal-utility-helpers-within-module-e4e6
description: "Adoption of lib/shopify Module for Commerce Data Integration: Internal Utility Helpers Within Module Layer"
metadata:
  actual:
    adr_id: e4e63b71-4958-4564-a6d3-3265ac97dfe6
    status: accepted
    candidate_group_id: aaff154b-2b92-4573-a820-1b4afa17030f
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of lib/shopify Module for Commerce Data Integration: Internal Utility Helpers Within Module Layer

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for:
- Application route handlers, dynamic page renderers, and sitemap generation modules requiring commerce catalog and page data.

### Rules

- **R-LM-001** MAY: Internal utility helpers within the module layer MAY extend domain data normalization routines provided they preserve exported interface contracts.

### Verify

- Discover and execute the project static analysis suite to verify that external commerce queries are restricted to the designated module.
- Discover and run the project automated test suite to confirm contract compatibility across all consuming routes.

**Accept when:**

- Static import analysis confirms zero direct third-party commerce network calls outside the designated module boundary.
- Automated integration verification suites pass against all routes consuming the centralized commerce module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis checks enforcing boundary restrictions on external commerce network invocations.
- Verified by: Mandatory peer review on pull requests touching application data layers and route implementations.
- Violation handling: Pull requests introducing unapproved direct network calls or alternative commerce clients are rejected at build gate.
- Violation handling: Identified architectural drift must be refactored into the centralized module before deployment approval.
- Exception process: Submit an architecture review request documenting why the centralized module cannot support the required query or transport protocol.
- Exception process: Obtain formal approval from the architecture review board before introducing alternative data access pathways.
</enforcement>
