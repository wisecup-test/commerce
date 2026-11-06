---
version: 2
name: cross-cutting-route-handlers-page-components-not-7df7
description: "Adoption of lib/shopify Module for Commerce Data Integration: Route Handlers Page Components Not Instantiate"
metadata:
  actual:
    adr_id: 7df789c4-b71f-4724-9ca9-8c8b0262fe2b
    status: accepted
    candidate_group_id: aaff154b-2b92-4573-a820-1b4afa17030f
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of lib/shopify Module for Commerce Data Integration: Route Handlers Page Components Not Instantiate

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for application route handlers, dynamic page renderers, and sitemap generation modules requiring commerce catalog and page data.

### Rules

- **R-SHOP-001** MUST_NOT: Route handlers and page components MUST NOT instantiate direct external network requests or alternative client wrappers to retrieve commerce entities managed by the lib/shopify module.

### Verify

```bash
# Discover and execute the project static analysis suite to verify external commerce queries are restricted to the designated module
# Discover and run the project automated test suite to confirm contract compatibility across all consuming routes
```

**Accept when:**
- Static import analysis confirms zero direct third-party commerce network calls outside the designated module boundary.
- Automated integration verification suites pass against all routes consuming the centralized commerce module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>