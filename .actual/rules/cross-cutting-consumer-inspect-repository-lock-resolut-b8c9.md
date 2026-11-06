---
version: 2
name: cross-cutting-consumer-inspect-repository-lock-resolut-b8c9
description: "Shopify Client Module (lib/shopify): Consumer Inspect Repository Lock Resolution Artifact"
metadata:
  actual:
    adr_id: b8c9f67d-7f3a-42da-b5d1-a0ab62bc4cf3
    status: accepted
    candidate_group_id: a61abcae-9d2e-4506-ac32-4166f16a7b12
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
---

# Shopify Client Module (lib/shopify): Consumer Inspect Repository Lock Resolution Artifact

Scope: applies to `app/search/**`.

These rules are ALWAYS ACTIVE for route handlers, page components, search views, and internal modules requiring e-commerce product and collection data.

### Rules

- **R-SHOPIFY-001** MUST: The consumer MUST inspect the repository lock or resolution artifact to determine the exact resolved version of any underlying integration SDK or HTTP client library before modifying module contracts.

### Verify

```bash
# Locate and execute the project's static analysis script to verify module import boundary compliance
# Discover and run the project's automated test runner from the root manifest
```

**Accept when:**
- All automated test suites covering catalog data fetching and search query resolution pass without regressions.
- Static analysis and module boundary checks confirm no direct external store API requests bypass the centralized module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration static analysis rules and automated integration tests enforce these module boundaries.
</enforcement>