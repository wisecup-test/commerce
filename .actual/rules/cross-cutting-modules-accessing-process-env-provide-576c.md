---
version: 2
name: cross-cutting-modules-accessing-process-env-provide-576c
description: "process.env Direct Runtime Configuration Sourcing: Modules Accessing Process Env Provide Fallback"
metadata:
  actual:
    adr_id: 576c2841-0794-4777-b71e-09e120f4d335
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

# process.env Direct Runtime Configuration Sourcing: Modules Accessing Process Env Provide Fallback

Scope: applies to `components/**`, `lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** SHOULD: Modules accessing process.env SHOULD provide fallback values or explicit guard checks when reading optional environment variables.

### Verify

```bash
# Discover and execute repository static analysis scripts to verify environment property access
# Discover and execute repository test suite
```

**Accept when:**
- All runtime environment variable references successfully resolve through process.env without throwing unexpected undefined exceptions.
- Static analysis and test suites pass without detecting unvalidated secret exposure across presentation components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>