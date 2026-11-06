---
version: 2
name: cross-cutting-when-entity-representation-changes-updat-a013
description: "Shared Fragment Module Architecture for Domain Queries: When Entity Representation Changes Updates Made"
metadata:
  actual:
    adr_id: a01335dd-1f2a-47ef-b88f-302847376536
    status: accepted
    candidate_group_id: d4161197-2aed-4594-b6a6-f649ed8252e5
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/queries/**
---

# Shared Fragment Module Architecture for Domain Queries: When Entity Representation Changes Updates Made

Scope: applies to `lib/shopify/queries/**`.

These rules are ALWAYS ACTIVE for data queries requiring domain entity field selections that recur across multiple query endpoints, and shared data access modules defining reusable entity representations.

### Rules

- **R-SFQ-001** MUST: When an entity representation changes, updates MUST be made directly within the corresponding shared fragment module to maintain consistency across all referencing queries.

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
Claude Code MUST NOT skip or defer verification. Automated static analysis, dependency boundary auditing, and code review checks during continuous integration enforce these rules.
</enforcement>