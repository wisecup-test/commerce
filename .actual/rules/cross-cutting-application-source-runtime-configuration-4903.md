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

These rules are ALWAYS ACTIVE for:
- External service integration clients communicating with third-party APIs.
- Server-side utility functions generating absolute URLs or domain-specific parameters.
- Metadata and dynamic visual asset generation components displaying site-level configuration.

### Rules

- **R-CEM-001** MUST: The application MUST source runtime configuration and environment variables through direct process.env property lookups across service clients, utility functions, and presentation components.

### Verify

- Discover and execute the repository static analysis scripts to verify that environment property access conforms to the prescribed pattern.
- Discover and execute the repository test suite to ensure that modules consuming process.env properties execute successfully under configured runtime environments.

**Accept when:**

- All runtime environment variable references successfully resolve through process.env without throwing unexpected undefined exceptions.
- Static analysis and test suites pass without detecting unvalidated secret exposure across presentation components.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Continuous integration automated verification inspecting environment variable usage and access boundaries.
- Verified by: Peer code review verifying that private credentials remain isolated from presentation components.
- Violation handling: Pull requests introducing unshielded or misplaced environment variable references will be blocked until remediated.
- Exception process: Exceptions requiring centralized validation abstractions must be documented in a design review and approved by the technical lead.
</enforcement>
