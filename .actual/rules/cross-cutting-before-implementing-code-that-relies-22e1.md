---
version: 2
name: cross-cutting-before-implementing-code-that-relies-22e1
description: "Server-Side Request Context and Cookie Management via next/headers: Before Implementing Code That Relies Next"
metadata:
  actual:
    adr_id: 22e16e34-bdf0-468d-83aa-c6ea89681115
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

# Server-Side Request Context and Cookie Management via next/headers: Before Implementing Code That Relies Next

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for:
- Server action handlers executing server-side state transitions.
- Service boundary modules and external client integrations that inspect incoming HTTP headers or manage session cookies.

### Rules

- **R-LM-001** MUST: Before implementing code that relies on next/headers, engineers MUST discover the repository dependency manifest and inspect the lock resolution artifact to identify the exact resolved version and verify accessor return signatures.

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
