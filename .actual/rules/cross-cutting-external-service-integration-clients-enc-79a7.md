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

These rules are ALWAYS ACTIVE for:
- External service integration clients communicating with third-party APIs.
- Server-side utility functions generating absolute URLs or domain-specific parameters.
- Metadata and dynamic visual asset generation components displaying site-level configuration.

### Rules

- **R-CEM-001** SHOULD: External service integration clients SHOULD encapsulate process.env configuration references within client initialization routines rather than scattering them across operational request methods.

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
