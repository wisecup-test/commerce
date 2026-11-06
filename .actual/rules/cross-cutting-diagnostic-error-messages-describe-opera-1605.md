---
version: 2
name: cross-cutting-diagnostic-error-messages-describe-opera-1605
description: "Console Error Logging for Server Operations: Diagnostic Error Messages Describe Operational Failure"
metadata:
  actual:
    adr_id: 16058a4e-f73f-4b84-909f-1a9301928af0
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

# Console Error Logging for Server Operations: Diagnostic Error Messages Describe Operational Failure

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for server-side actions, data layer integration boundaries, and webhook revalidation handlers encountering caught exceptions or invalid request states.

### Rules

- **R-DIAG-001** SHOULD: Diagnostic error messages describe the operational failure context clearly without logging secret credentials or session tokens.

### Verify

```bash
# Discover and run the project static analysis and linting scripts to verify error handling compliance across server modules.
# Discover and execute the project test runner to validate that error branches emit diagnostic logs as expected.
```

**Accept when:**
- Static analysis confirms all designated server-side error paths and validation guard branches route error diagnostics to the standard error stream.
- Automated testing verifies that failed transactional operations emit diagnostic records without unhandled promise rejections.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>