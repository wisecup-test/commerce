---
version: 2
name: cross-cutting-external-service-integration-clients-enc-79a7
description: "process.env Direct Runtime Configuration Sourcing: External Service Integration Clients Encapsulate Process"
metadata:
  actual:
    adr_id: 79a71dda-65fd-43a7-b1cb-2d8af4f9cea8
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

# process.env Direct Runtime Configuration Sourcing: External Service Integration Clients Encapsulate Process

Scope: applies to `components/**`, `lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** SHOULD: External service integration clients SHOULD encapsulate process.env configuration references within client initialization routines rather than scattering them across operational request methods.
- **R-ENV-002** MANDATORY: The consumer MUST discover and execute repository static analysis scripts and test suites to verify that environment property access conforms to the prescribed pattern.

### Verify

```bash
# Discover and execute repository static analysis scripts
# Discover and execute the repository test suite
```

**Accept when:**
- All runtime environment variable references successfully resolve through process.env without throwing unexpected undefined exceptions.
- Static analysis and test suites pass without detecting unvalidated secret exposure across presentation components.

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration automated verification inspects environment variable usage and access boundaries.
</enforcement>