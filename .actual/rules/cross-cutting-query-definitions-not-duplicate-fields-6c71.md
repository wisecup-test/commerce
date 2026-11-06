---
version: 2
name: cross-cutting-query-definitions-not-duplicate-fields-6c71
description: "Shared Fragment Module Architecture for Domain Queries: Query Definitions Not Duplicate Fields That"
metadata:
  actual:
    adr_id: 6c711ac1-11c6-4e9a-8c7d-9db711b72823
    status: accepted
    candidate_group_id: d4161197-2aed-4594-b6a6-f649ed8252e5
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/queries/**
---

# Shared Fragment Module Architecture for Domain Queries: Query Definitions Not Duplicate Fields That

Scope: applies to `lib/shopify/queries/**`.

These rules are ALWAYS ACTIVE for all data queries requiring domain entity field selections that recur across multiple query endpoints, and shared data access modules defining reusable entity representations.

### Rules

- **R-QRY-001** MUST_NOT: Query definitions MUST NOT duplicate fields that are already defined and exported by an existing shared fragment module.

### Verify

```bash
# Find the project verification script for module import analysis and execute it to verify that query modules reference shared fragment modules rather than inline definitions.
# Discover and run the project type-checking test suite to confirm query modules integrate fragment definitions cleanly without syntax or import errors.
```

**Accept when:**
- All data query definitions compose shared fragment modules for common entity representations without inline field duplication.
- Automated static analysis and type checks pass across all query and fragment modules.
- Dependency resolution confirms all fragment imports resolve cleanly within the project architecture.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>