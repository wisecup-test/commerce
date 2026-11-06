---
version: 2
name: cross-cutting-query-definitions-requesting-entity-meta-0125
description: "Adoption of Shared SEO Fragment Module for Query Composition: Query Definitions Requesting Entity Metadata Compose"
metadata:
  actual:
    adr_id: 01256ed3-7e3a-42cb-924d-4d1fc86c52eb
    status: accepted
    candidate_group_id: 6c3b4e2b-88b7-4462-a4da-7a0f7e4a8746
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/queries/**
---

# Adoption of Shared SEO Fragment Module for Query Composition: Query Definitions Requesting Entity Metadata Compose

Scope: applies to `lib/shopify/queries/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SEO-001** MUST: Query definitions requesting entity metadata MUST compose fields using the shared SEO fragment module rather than declaring duplicate inline metadata selections.

### Verify

```bash
# Discover and execute the repository type-checking and schema validation scripts
# Discover and run the project test suite
```

**Accept when:**
- Static type checking and query schema validation pass with zero errors across all query definitions referencing the shared SEO fragment module.
- All repository test suites asserting query document construction and data retrieval succeed.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>