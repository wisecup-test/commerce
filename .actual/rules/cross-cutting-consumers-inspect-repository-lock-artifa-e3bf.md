---
version: 2
name: cross-cutting-consumers-inspect-repository-lock-artifa-e3bf
description: "Shared Fragment Module Architecture for Domain Queries: Consumers Inspect Repository Lock Artifact Verify"
metadata:
  actual:
    adr_id: e3bfc1d8-235e-4068-8fd2-6027de35a97e
    status: accepted
    candidate_group_id: d4161197-2aed-4594-b6a6-f649ed8252e5
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - lib/shopify/queries/**
---

# Shared Fragment Module Architecture for Domain Queries: Consumers Inspect Repository Lock Artifact Verify

Scope: applies to `lib/shopify/queries/**`.

These rules are ALWAYS ACTIVE for data queries requiring domain entity field selections that recur across multiple query endpoints and shared data access modules defining reusable entity representations.

### Rules

- **R-SFM-001** MUST: Consumers MUST inspect the repository lock artifact to verify the exact resolved version of any supporting libraries before implementing or modifying query execution infrastructure.
- **R-SFM-002** MUST: Query modules MUST compose standardized data contracts cleanly by centralizing common entity shapes into dedicated fragment modules.
- **R-SFM-003** MUST: Consumers MUST derive all tool names, file names, commands, package managers, and version numbers from the project repository.
- **R-SFM-004** MUST: Before writing code that uses a versioned library, developers/consumers MUST find the dependency manifest, identify the build tool, inspect the repository lock or resolution artifact for exact resolved versions, look up official documentation for that exact version, confirm API existence, and re-verify per dependency at the point of use.
- **R-SFM-005** SHOULD: Decompose shared entity representations into granular sub-fragments when differing query contexts require distinct subsets of fields.
- **R-SFM-006** SHOULD: Organize fragment modules by domain entity concerns adjacent to corresponding query modules within the data access architecture.

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
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>