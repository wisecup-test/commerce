---
version: 2
name: cross-cutting-commerce-catalog-operations-product-sear-b6a2
description: "Shopify Client Module (lib/shopify): Commerce Catalog Operations Product Search Queries"
metadata:
  actual:
    adr_id: b6a211a5-0966-4c62-ac6e-65c052933127
    status: accepted
    candidate_group_id: a61abcae-9d2e-4506-ac32-4166f16a7b12
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
---

# Shopify Client Module (lib/shopify): Commerce Catalog Operations Product Search Queries

Scope: applies to `app/search/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** MUST: All e-commerce catalog operations, product search queries, and collection retrievals MUST be executed through the dedicated lib/shopify client module rather than direct external API calls from presentation routes.

### Verify

```bash
# Inspect the repository configuration to locate and execute the project's static analysis script to verify module import boundary compliance.
# Discover and run the project's automated test runner from the root manifest to validate catalog retrieval and sorting query behavior.
```

**Accept when:**
- All automated test suites covering catalog data fetching and search query resolution pass without regressions.
- Static analysis and module boundary checks confirm no direct external store API requests bypass the centralized module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>