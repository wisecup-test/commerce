---
version: 2
name: cross-cutting-presentation-components-route-handlers-n-291c
description: "Shopify Client Module (lib/shopify): Presentation Components Route Handlers Not Instantiate"
metadata:
  actual:
    adr_id: 291c74ba-e89a-471c-ac6e-5ff9ec74c865
    status: accepted
    candidate_group_id: a61abcae-9d2e-4506-ac32-4166f16a7b12
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
---

# Shopify Client Module (lib/shopify): Presentation Components Route Handlers Not Instantiate

Scope: applies to `app/search/**`.

These rules are ALWAYS ACTIVE for route handlers, page components, and search views requiring e-commerce product and collection data, and internal modules managing catalog queries, collection filtering, and product sorting.

### Rules

- **R-SHOPIFY-001** MUST_NOT: Presentation components and route handlers MUST NOT instantiate custom API clients or issue direct network requests to the e-commerce service provider.

### Verify

```bash
# Locate and execute the project's static analysis script to verify module import boundary compliance
# Discover and run the project's automated test runner from the root manifest to validate catalog retrieval and sorting query behavior
```

**Accept when:**
- All automated test suites covering catalog data fetching and search query resolution pass without regressions.
- Static analysis and module boundary checks confirm no direct external store API requests bypass the centralized module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>