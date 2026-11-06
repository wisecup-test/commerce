---
version: 2
name: cross-cutting-modules-consuming-lib-shopify-module-c44b
description: "Adoption of lib/shopify Module for Commerce Data Integration: Modules Consuming Lib Shopify Module Handle"
metadata:
  actual:
    adr_id: c44bea66-c969-470b-9ecc-85b38d8efec5
    status: accepted
    candidate_group_id: aaff154b-2b92-4573-a820-1b4afa17030f
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of lib/shopify Module for Commerce Data Integration: Modules Consuming Lib Shopify Module Handle

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** SHOULD: Modules consuming the lib/shopify module SHOULD handle data access exceptions at the route boundary to provide resilient fallback representations.

### Verify

```bash
# Discover and execute the project static analysis suite to verify that external commerce queries are restricted to the designated module.
# Discover and run the project automated test suite to confirm contract compatibility across all consuming routes.
```

**Accept when:**
- Static import analysis confirms zero direct third-party commerce network calls outside the designated module boundary.
- Automated integration verification suites pass against all routes consuming the centralized commerce module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>