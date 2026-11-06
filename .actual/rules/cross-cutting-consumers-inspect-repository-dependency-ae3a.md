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

These rules are ALWAYS ACTIVE for server components, route handlers, server actions, metadata generators, and UI components querying, mutating, or rendering e-commerce domain resources.

### Rules

- **R-SHOPIFY-001** MUST: Consumers MUST inspect the repository dependency resolution artifact to determine the authoritative resolved version of all supporting runtime dependencies before executing operations.

### Verify

```bash
# Discover and run the project static analysis and type-checking scripts
# Discover and execute the automated test suite
# Discover and run the dependency audit script
```

**Accept when:**
- Static analysis confirms zero direct external commerce API network requests across presentation components and route handlers.
- All unit and integration test suites validating the centralized commerce module pass without errors.
- Dependency resolution checks verify that external commerce runtime dependencies match authoritative lock file definitions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated continuous integration checks, static analysis, type checking, and test suites verify adherence to domain module architectural boundaries.
</enforcement>