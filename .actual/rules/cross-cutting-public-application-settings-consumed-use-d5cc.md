---
version: 2
name: cross-cutting-public-application-settings-consumed-use-d5cc
description: "Next.js Secrets Management and Runtime Environment Access: Public Application Settings Consumed User Interface"
metadata:
  actual:
    adr_id: d5cc76a1-37c1-4ff0-a3fc-85be0e38b431
    status: accepted
    candidate_group_id: 6554969c-07a0-4c49-9b32-231648e13714
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
      - lib/**
---

# Next.js Secrets Management and Runtime Environment Access: Public Application Settings Consumed User Interface

Scope: applies to `components/**`, `lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NXT-001** SHOULD: Public application settings consumed by user interface components and metadata generators SHOULD be scoped to non-sensitive environment variables.

### Verify

```bash
# Discover and execute the repository static analysis script to verify environment variables are accessed strictly through approved patterns.
# Discover and execute the repository build script to verify no server-only secrets leak into client bundle artifacts.
# Discover and run the test runner from the dependency manifest to validate secret comparison and revalidation error handling.
```

**Accept when:**
- Static analysis confirms no sensitive environment variables are imported or referenced within client-side component trees.
- The project build succeeds without emitting warnings regarding credential leakage into browser bundles.
- Automated tests pass validating that external client requests include required credential headers and that invalid webhook secrets are rejected.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>