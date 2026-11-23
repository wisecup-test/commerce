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

These rules are ALWAYS ACTIVE for:
- Data queries requiring domain entity field selections that recur across multiple query endpoints.
- Shared data access modules defining reusable entity representations.

### Rules

- **R-LM-001** MUST: Consumers MUST inspect the repository lock artifact to verify the exact resolved version of any supporting libraries before implementing or modifying query execution infrastructure.

### Verify

- Find the project verification script for module import analysis and execute it to verify that query modules reference shared fragment modules rather than inline definitions.
- Discover and run the project type-checking test suite to confirm query modules integrate fragment definitions cleanly without syntax or import errors.
- Query the version control history to verify that modifications to shared entity shapes update the corresponding fragment module rather than individual query definitions.

**Accept when:**

- All data query definitions compose shared fragment modules for common entity representations without inline field duplication.
- Automated static analysis and type checks pass across all query and fragment modules.
- Dependency resolution confirms all fragment imports resolve cleanly within the project architecture.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis and code review checks during continuous integration.
- Verified by: Dependency boundary auditing to verify that query modules consume centralized fragment modules.
- Violation handling: Pull requests containing inline duplicates of existing fragment definitions are blocked until refactored into shared fragments.
- Violation handling: Unregistered query field structures are flagged for architectural review.
- Exception process: Architectural review submission detailing the performance or structural rationale for an isolated query projection.
</enforcement>
