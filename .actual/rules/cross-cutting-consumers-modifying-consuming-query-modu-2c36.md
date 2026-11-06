---
version: 2
name: cross-cutting-consumers-modifying-consuming-query-modu-2c36
description: "Adoption of Shared SEO Fragment Module for Query Composition: Consumers Modifying Consuming Query Modules Verify"
metadata:
  actual:
    adr_id: 2c3677f9-00e3-4db3-a4a4-471ddb445f29
    status: accepted
    candidate_group_id: 6c3b4e2b-88b7-4462-a4da-7a0f7e4a8746
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/queries/**
---

# Adoption of Shared SEO Fragment Module for Query Composition: Consumers Modifying Consuming Query Modules Verify

Scope: applies to `lib/shopify/queries/**`.

These rules are ALWAYS ACTIVE for all entity and collection query definitions requiring search engine optimization metadata selections.

### Rules

- **R-SEO-001** MUST: Consumers modifying or consuming query modules MUST verify project lock artifacts to validate that runtime dependencies match resolved environment declarations.

### Verify

```bash
# Discover and execute repository type-checking and schema validation scripts
# Discover and run the project test suite
```

**Accept when:**
- Static type checking and query schema validation pass with zero errors across all query definitions referencing the shared SEO fragment module.
- All repository test suites asserting query document construction and data retrieval succeed.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>