---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-ae3a
description: "Centralized Commerce Data Access and Domain Module via lib/shopify: Consumers Inspect Repository Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: ae3ad0d6-ca9f-4567-b68b-65e4ed47ef15
    status: accepted
    candidate_group_id: 200a08a3-c441-4571-bdcc-aa8c14b01867
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
      - components/**
---

# Centralized Commerce Data Access and Domain Module via lib/shopify: Consumers Inspect Repository Dependency Resolution Artifact

Scope: applies to `app/**`, `components/**`.

These rules are ALWAYS ACTIVE for:
- Server components, route handlers, server actions, and metadata generators querying or mutating e-commerce domain resources.
- Components rendering catalog items, navigation links, cart state, or dynamic promotional displays.

### Rules

- **R-LM-001** MUST: Consumers MUST inspect the repository dependency resolution artifact to determine the authoritative resolved version of all supporting runtime dependencies before executing operations.

### Verify

- Discover and run the project static analysis and type-checking scripts to verify that all commerce imports reference the centralized module.
- Discover and execute the automated test suite to ensure commerce queries, mutations, and revalidation workflows pass without errors.
- Discover and run the dependency audit script to confirm resolution artifacts match project manifest constraints.

**Accept when:**

- Static analysis confirms zero direct external commerce API network requests across presentation components and route handlers.
- All unit and integration test suites validating the centralized commerce module pass without errors.
- Dependency resolution checks verify that external commerce runtime dependencies match authoritative lock file definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated continuous integration checks executing static analysis, type checking, and test suites.
- Verified by: Peer code review validating adherence to domain module architectural boundaries.
- Violation handling: Pull requests containing direct external commerce endpoint calls or bypassing the centralized module will be blocked.
- Violation handling: Violating code must be refactored to consume the centralized domain interfaces prior to merge approval.
- Exception process: Developers must submit an architectural exception request detailing technical blockers to the engineering leadership team.
- Exception process: Approved temporary exceptions must include a deprecation milestone and a refactoring task tracked in the issue tracker.
</enforcement>
