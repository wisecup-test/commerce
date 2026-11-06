---
version: 2
name: cross-cutting-centralized-integration-module-normalize-e4b3
description: "Shopify Client Module (lib/shopify): Centralized Integration Module Normalize Provider Responses"
metadata:
  actual:
    adr_id: e4b31137-6c14-459a-a0e2-eed30e251461
    status: accepted
    candidate_group_id: a61abcae-9d2e-4506-ac32-4166f16a7b12
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
---

# Shopify Client Module (lib/shopify): Centralized Integration Module Normalize Provider Responses

Scope: applies to `app/search/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SHOPIFY-001** SHOULD: The centralized integration module SHOULD normalize all provider responses into strongly typed internal domain representations before exposing them to layout and grid components.

### Verify

```bash
# Inspect the repository configuration to locate and execute the project's static analysis script to verify module import boundary compliance.
# Discover and run the project's automated test runner from the root manifest to validate catalog retrieval and sorting query behavior.
```

**Accept when:**
- All automated test suites covering catalog data fetching and search query resolution pass without regressions.
- Static analysis and module boundary checks confirm no direct external store API requests bypass the centralized module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>