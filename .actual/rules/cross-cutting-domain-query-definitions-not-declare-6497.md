---
version: 2
name: cross-cutting-domain-query-definitions-not-declare-6497
description: "Adoption of Shared SEO Fragment Module for Query Composition: Domain Query Definitions Not Declare Custom"
metadata:
  actual:
    adr_id: 64972eb4-7498-4467-8beb-7a2545075583
    status: accepted
    candidate_group_id: 6c3b4e2b-88b7-4462-a4da-7a0f7e4a8746
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/queries/**
---

# Adoption of Shared SEO Fragment Module for Query Composition: Domain Query Definitions Not Declare Custom

Scope: applies to `lib/shopify/queries/**`.

These rules are ALWAYS ACTIVE for all entity and collection query definitions requiring search engine optimization metadata selections.

### Rules

- **R-SEO-001** MUST_NOT: Domain query definitions MUST NOT declare custom or ad-hoc metadata selections that bypass the shared SEO fragment module.

### Verify

```bash
# Discover and execute the repository type-checking and schema validation scripts
# Discover and run the project test suite to verify query construction and execution
```

**Accept when:**
- Static type checking and query schema validation pass with zero errors across all query definitions referencing the shared SEO fragment module.
- All repository test suites asserting query document construction and data retrieval succeed.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>