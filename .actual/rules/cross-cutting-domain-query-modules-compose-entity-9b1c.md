---
version: 2
name: cross-cutting-domain-query-modules-compose-entity-9b1c
description: "Shared Fragment Module Architecture for Domain Queries: Domain Query Modules Compose Entity Field"
metadata:
  actual:
    adr_id: 9b1cf2a9-8d08-4910-a883-136b09883774
    status: accepted
    candidate_group_id: d4161197-2aed-4594-b6a6-f649ed8252e5
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/queries/**
---

# Shared Fragment Module Architecture for Domain Queries: Domain Query Modules Compose Entity Field

Scope: applies to `lib/shopify/queries/**`.

These rules are ALWAYS ACTIVE for all data queries requiring domain entity field selections that recur across multiple query endpoints, and shared data access modules defining reusable entity representations.

### Rules

- **R-DOM-001** MUST: Domain query modules MUST compose entity field selections using shared fragment modules rather than defining redundant inline entity representations.

### Verify

```bash
# Find the project verification script for module import analysis and execute it to verify that query modules reference shared fragment modules rather than inline definitions.
# Discover and run the project type-checking test suite to confirm query modules integrate fragment definitions cleanly without syntax or import errors.
# Query the version control history to verify that modifications to shared entity shapes update the corresponding fragment module rather than individual query definitions.
```

**Accept when:**
- All data query definitions compose shared fragment modules for common entity representations without inline field duplication.
- Automated static analysis and type checks pass across all query and fragment modules.
- Dependency resolution confirms all fragment imports resolve cleanly within the project architecture.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis and code review checks during continuous integration enforce these rules, and pull requests containing inline duplicates of existing fragment definitions are blocked.
</enforcement>