---
version: 2
name: cross-cutting-application-source-runtime-configuration-4903
description: "process.env Direct Runtime Configuration Sourcing: Application Source Runtime Configuration Environment Variables"
metadata:
  actual:
    adr_id: 4903b545-5449-4f72-91fd-6097b2b8927e
    status: accepted
    candidate_group_id: 150319c5-2c14-4765-bd2d-f9e2bc22fdd3
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
      - lib/**
---

# process.env Direct Runtime Configuration Sourcing: Application Source Runtime Configuration Environment Variables

Scope: applies to `components/**`, `lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: The application MUST source runtime configuration and environment variables through direct process.env property lookups across service clients, utility functions, and presentation components.

### Verify

```bash
# Discover and execute repository static analysis scripts to verify environment property access
# Discover and execute repository test suites to ensure modules consuming process.env execute successfully
```

**Accept when:**
- All runtime environment variable references successfully resolve through process.env without throwing unexpected undefined exceptions.
- Static analysis and test suites pass without detecting unvalidated secret exposure across presentation components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>