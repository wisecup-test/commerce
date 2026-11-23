---
version: 2
name: cross-cutting-modules-not-attempt-cookie-mutation-7dcb
description: "Server-Side Request Context and Cookie Management via next/headers: Modules Not Attempt Cookie Mutation Outside"
metadata:
  actual:
    adr_id: 7dcbbca5-2de1-4da0-8822-807c390ecf53
    status: accepted
    candidate_group_id: 0e0e1f68-e5a9-4e8a-aec5-00713a779cbb
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - lib/shopify/**
---

# Server-Side Request Context and Cookie Management via next/headers: Modules Not Attempt Cookie Mutation Outside

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for:
- Server action handlers executing server-side state transitions.
- Service boundary modules and external client integrations that inspect incoming HTTP headers or manage session cookies.

### Rules

- **R-LM-001** MUST_NOT: Modules MUST NOT attempt cookie mutation outside of designated server action mutations or framework-supported route execution lifecycles.

### Verify

- Discover and execute the project type-checking and linting scripts from the repository configuration to verify all next/headers imports and accessor signatures satisfy contract requirements.
- Discover and run the automated test suite configured in the repository manifest to verify server actions and integration handlers appropriately handle header and cookie state.

**Accept when:**

- Static analysis and type validation pass with zero diagnostic errors across all modules importing next/headers.
- Unit and integration test suites validating server action cookie manipulation and header extraction pass completely.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis and type validation in continuous integration pipelines.
- Verified by: Peer code reviews verifying accessor encapsulation and context boundaries.
- Violation handling: Continuous integration build failures on unhandled promise rejections or invalid accessor signatures.
- Violation handling: Mandatory code review corrections for unauthorized header mutations or improper context access outside server boundaries.
- Exception process: Documented architectural exemption approved by engineering leads when custom transport adapters require raw request inspection.
</enforcement>
