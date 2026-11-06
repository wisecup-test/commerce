---
version: 2
name: cross-cutting-route-parameters-defining-catalog-sortin-30d2
description: "Shopify Client Module (lib/shopify): Route Parameters Defining Catalog Sorting Criteria"
metadata:
  actual:
    adr_id: 30d22d5a-ebf8-4828-a0ed-f837f86fb725
    status: accepted
    candidate_group_id: a61abcae-9d2e-4506-ac32-4166f16a7b12
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
---

# Shopify Client Module (lib/shopify): Route Parameters Defining Catalog Sorting Criteria

Scope: applies to `app/search/**`.

These rules are ALWAYS ACTIVE for route handlers, page components, search views, and internal catalog query modules requiring e-commerce product and collection data.

### Rules

- **R-SHOP-001** SHOULD: Route parameters defining catalog sorting criteria SHOULD be validated against established sorting constants before invoking data queries on the integration module.

### Verify

```bash
# Locate and execute the project's static analysis script to verify module import boundary compliance
# Discover and run the project's automated test runner from the root manifest to validate catalog retrieval and sorting query behavior
```

**Accept when:**
- All automated test suites covering catalog data fetching and search query resolution pass without regressions.
- Static analysis and module boundary checks confirm no direct external store API requests bypass the centralized module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static analysis rules and automated integration test suites verify adherence to centralized data client module patterns, blocking merges on violations.
</enforcement>