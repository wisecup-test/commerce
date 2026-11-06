---
version: 2
name: cross-cutting-shared-seo-fragment-module-encapsulate-1e66
description: "Adoption of Shared SEO Fragment Module for Query Composition: Shared Seo Fragment Module Encapsulate Standardized"
metadata:
  actual:
    adr_id: 1e664035-ff8b-4692-92a3-d01a620d59e7
    status: accepted
    candidate_group_id: 6c3b4e2b-88b7-4462-a4da-7a0f7e4a8746
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/queries/**
---

# Adoption of Shared SEO Fragment Module for Query Composition: Shared Seo Fragment Module Encapsulate Standardized

Scope: applies to `lib/shopify/queries/**`.

These rules are ALWAYS ACTIVE for all entity and collection query definitions requiring search engine optimization metadata selections.

### Rules

- **R-SEO-001** MUST: The shared SEO fragment module MUST encapsulate all standardized search engine optimization field definitions required across domain query boundaries.

### Verify

```bash
# Discover and execute the repository type-checking and schema validation scripts
# Discover and run the project test suite to verify query module construction and execution
```

**Accept when:**
- Static type checking and query schema validation pass with zero errors across all query definitions referencing the shared SEO fragment module.
- All repository test suites asserting query document construction and data retrieval succeed.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>