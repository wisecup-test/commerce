---
version: 2
name: cross-cutting-shared-fragment-modules-encapsulate-sing-27c0
description: "Shared Fragment Module Architecture for Domain Queries: Shared Fragment Modules Encapsulate Single Domain"
metadata:
  actual:
    adr_id: 27c03720-21e3-4de3-b017-e80131e57cc1
    status: accepted
    candidate_group_id: d4161197-2aed-4594-b6a6-f649ed8252e5
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/queries/**
---

# Shared Fragment Module Architecture for Domain Queries: Shared Fragment Modules Encapsulate Single Domain

Scope: applies to `lib/shopify/queries/**`.

These rules are ALWAYS ACTIVE for data queries requiring domain entity field selections that recur across multiple query endpoints, and shared data access modules defining reusable entity representations.

### Rules

- **R-SFM-001** MUST: Shared fragment modules MUST encapsulate single domain entity shapes and export them for reuse across dependent query modules.

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
Claude Code MUST NOT skip or defer verification.
</enforcement>