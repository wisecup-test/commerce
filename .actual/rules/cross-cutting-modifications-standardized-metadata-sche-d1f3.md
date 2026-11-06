---
version: 2
name: cross-cutting-modifications-standardized-metadata-sche-d1f3
description: "Adoption of Shared SEO Fragment Module for Query Composition: Modifications Standardized Metadata Schemas Introduced Directly"
metadata:
  actual:
    adr_id: d1f33172-37e2-4eb4-ab15-6035ebeae8de
    status: accepted
    candidate_group_id: 6c3b4e2b-88b7-4462-a4da-7a0f7e4a8746
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/queries/**
---

# Adoption of Shared SEO Fragment Module for Query Composition: Modifications Standardized Metadata Schemas Introduced Directly

Scope: applies to `lib/shopify/queries/**`.

These rules are ALWAYS ACTIVE for entity and collection query definitions requiring search engine optimization metadata selections.

### Rules

- **R-SEO-001** SHOULD: Modifications to standardized metadata schemas SHOULD be introduced directly within the shared SEO fragment module to propagate updates across all consuming queries.

### Verify

```bash
# Discover and execute the repository type-checking, schema validation, and test suite commands
# Example discovery:
# npm test
# npx tsc --noEmit
```

**Accept when:**
- Static type checking and query schema validation pass with zero errors across all query definitions referencing the shared SEO fragment module.
- All repository test suites asserting query document construction and data retrieval succeed.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>