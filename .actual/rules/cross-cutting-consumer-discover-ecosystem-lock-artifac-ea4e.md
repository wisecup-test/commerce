---
version: 2
name: cross-cutting-consumer-discover-ecosystem-lock-artifac-ea4e
description: "Next.js Secrets Management and Runtime Environment Access: Consumer Discover Ecosystem Lock Artifact Project"
metadata:
  actual:
    adr_id: ea4e45fd-d1da-4a3d-8ba9-c38e1c37ba10
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

# Next.js Secrets Management and Runtime Environment Access: Consumer Discover Ecosystem Lock Artifact Project

Scope: applies to `components/**`, `lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: The consumer MUST discover the ecosystem lock artifact in the project repository and resolve the exact locked version of the framework and runtime dependencies before implementing or modifying environment configuration interfaces.

### Verify

```bash
# Discover and execute the repository static analysis script to verify environment variables are accessed strictly through approved patterns.
# Discover and execute the repository build script to verify that no server-only secrets are leaked into client bundle artifacts.
# Discover the repository test runner from the dependency manifest and run the suite to validate secret comparison and revalidation error handling.
```

**Accept when:**
- Static analysis confirms no sensitive environment variables are imported or referenced within client-side component trees.
- The project build succeeds without emitting warnings regarding credential leakage into browser bundles.
- Automated tests pass validating that external client requests include required credential headers and that invalid webhook secrets are rejected.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>