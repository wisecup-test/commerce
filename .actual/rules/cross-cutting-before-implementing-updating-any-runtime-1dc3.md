---
version: 2
name: cross-cutting-before-implementing-updating-any-runtime-1dc3
description: "process.env Direct Runtime Configuration Sourcing: Before Implementing Updating Any Runtime Configuration"
metadata:
  actual:
    adr_id: 1dc3bcda-aa21-4452-9811-4920367bae63
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

# process.env Direct Runtime Configuration Sourcing: Before Implementing Updating Any Runtime Configuration

Scope: applies to `components/**`, `lib/**`.

These rules are ALWAYS ACTIVE for external service integration clients communicating with third-party APIs, server-side utility functions generating absolute URLs or domain-specific parameters, and metadata and dynamic visual asset generation components displaying site-level configuration.

### Rules

- **R-ENV-001** MUST: Before implementing or updating any runtime configuration dependencies, the consumer MUST inspect the repository lock artifact to determine the exact resolved dependency versions.

### Verify

```bash
# Discover and execute repository static analysis scripts to verify environment property access
# Discover and execute repository test suite to ensure modules consuming process.env properties execute successfully
```

**Accept when:**
- All runtime environment variable references successfully resolve through process.env without throwing unexpected undefined exceptions.
- Static analysis and test suites pass without detecting unvalidated secret exposure across presentation components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>