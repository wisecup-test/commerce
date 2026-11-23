---
version: 2
name: cross-cutting-application-routes-dynamic-page-handlers-aab7
description: "Adoption of lib/shopify Module for Commerce Data Integration: Application Routes Dynamic Page Handlers Site"
metadata:
  actual:
    adr_id: aab7267e-72f7-45b0-9f0d-567739a26fb8
    status: accepted
    candidate_group_id: aaff154b-2b92-4573-a820-1b4afa17030f
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of lib/shopify Module for Commerce Data Integration: Application Routes Dynamic Page Handlers Site

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for:
- Application route handlers, dynamic page renderers, and sitemap generation modules requiring commerce catalog and page data.

### Rules

- **R-LM-001** MUST: All application routes, dynamic page handlers, and site metadata generators requiring commerce domain operations MUST access external commerce services exclusively through the lib/shopify module.

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
