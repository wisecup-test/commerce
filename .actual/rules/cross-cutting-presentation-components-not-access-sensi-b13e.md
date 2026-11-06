---
version: 2
name: cross-cutting-presentation-components-not-access-sensi-b13e
description: "process.env Direct Runtime Configuration Sourcing: Presentation Components Not Access Sensitive Credentials"
metadata:
  actual:
    adr_id: b13ea1ce-580a-4b62-8228-f0b49b793227
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

# process.env Direct Runtime Configuration Sourcing: Presentation Components Not Access Sensitive Credentials

Scope: applies to `components/**`, `lib/**`.

These rules are ALWAYS ACTIVE for external service integration clients, server-side utility functions generating absolute URLs, and metadata/dynamic visual asset generation components.

### Rules

- **R-ENV-001** MUST_NOT: Presentation components MUST_NOT access sensitive credentials or private API tokens directly through process.env.

### Verify

```bash
# Discover and execute repository static analysis scripts to verify environment property access
# Discover and execute repository test suites to ensure modules consuming process.env properties execute successfully
```

**Accept when:**
- All runtime environment variable references successfully resolve through process.env without throwing unexpected undefined exceptions.
- Static analysis and test suites pass without detecting unvalidated secret exposure across presentation components.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated verification and peer code review inspect environment variable usage and access boundaries.
</enforcement>