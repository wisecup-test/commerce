---
version: 2
name: cross-cutting-server-side-action-handlers-external-66f8
description: "Console Error Logging for Server Operations: Server Side Action Handlers External Service"
metadata:
  actual:
    adr_id: 66f81017-537f-4cdf-b297-c9f20c21868f
    status: accepted
    candidate_group_id: 593f3998-bd25-40e8-b4c5-ef6bf73ae5f1
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
      - lib/shopify/**
---

# Console Error Logging for Server Operations: Server Side Action Handlers External Service

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for:
- Server-side actions, data layer integration boundaries, and webhook revalidation handlers encountering caught exceptions or invalid request states.

### Rules

- **R-L-001** MUST: Server-side action handlers and external service integration modules MUST emit operational failures and validation rejections using runtime console.error.

### Verify

- Discover and run the project static analysis and linting scripts to verify error handling compliance across server modules.
- Discover and execute the project test runner to validate that error branches emit diagnostic logs as expected.

**Accept when:**

- Static analysis confirms all designated server-side error paths and validation guard branches route error diagnostics to the standard error stream.
- Automated testing verifies that failed transactional operations emit diagnostic records without unhandled promise rejections.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis checks in continuous integration pipelines.
- Verified by: Peer review during pull request evaluations of server-side error handlers.
- Violation handling: Pull requests with unlogged caught exceptions or non-compliant logging calls are blocked until corrected.
- Violation handling: Static analysis rule violations fail build steps automatically.
- Exception process: Submit an architectural review request outlining why a module requires structured output or silent fallback handling.
- Exception process: Document the approved logging variance in the module documentation prior to merging.
</enforcement>
