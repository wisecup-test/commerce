---
version: 2
name: cross-cutting-before-adopting-integrating-any-versione-bfe3
description: "Console Error Logging for Server Operations: Before Adopting Integrating Any Versioned External"
metadata:
  actual:
    adr_id: bfe35ab5-c388-4290-970c-973ab21c16ff
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

# Console Error Logging for Server Operations: Before Adopting Integrating Any Versioned External

Scope: applies to `components/cart/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for server-side actions, data layer integration boundaries, and webhook revalidation handlers encountering caught exceptions or invalid request states.

### Rules

- **R-LOG-001** MUST: Before adopting or integrating any versioned external logging library, the consumer MUST inspect the repository lock artifact and verify the exact resolved version according to the mandatory lock-version grounding policy.
- **R-LOG-002** MUST: Follow the MANDATORY DISCOVERY POLICY to derive all tool names, file names, commands, package managers, and version numbers from the project repository rather than assumptions.
- **R-LOG-003** MUST: Execute the LOCK-VERSION GROUNDING procedure in order (manifest -> build tool -> lock/resolution artifact -> documentation lookup -> API confirmation -> version-sensitive re-check) before writing code that uses a versioned library.
- **R-LOG-004** MUST: Sanitize all logged variables and error context objects prior to error emission to prevent credential or sensitive token/header leakage.
- **R-LOG-005** MUST: Pass error instances directly to preserve native runtime stack trace information.

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