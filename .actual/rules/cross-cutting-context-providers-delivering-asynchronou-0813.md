---
version: 2
name: cross-cutting-context-providers-delivering-asynchronou-0813
description: "React Promise Unwrapping via use API at Context Boundaries: Context Providers Delivering Asynchronous Resources Encapsulate"
metadata:
  actual:
    adr_id: 0813866a-f5d7-4ef8-8db5-3ff2ec656ddd
    status: accepted
    candidate_group_id: 20960f3d-e58a-4d3f-bbd1-4049364fd937
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
---

# React Promise Unwrapping via use API at Context Boundaries: Context Providers Delivering Asynchronous Resources Encapsulate

Scope: applies to `components/cart/**`.

These rules are ALWAYS ACTIVE for:
- Client components and context consumer hooks consuming asynchronous context promises.
- Context providers encapsulating asynchronous resource promises for subtree consumption.

### Rules

- **R-ISP-001** SHOULD: Context providers delivering asynchronous resources SHOULD encapsulate the underlying promise within dedicated context objects to separate promise delivery from synchronous value access.

### Verify

- Discover the project test runner from the repository manifest and execute component test suites verifying context promise resolution.
- Discover the static analysis verification command from the repository configuration and validate hook usage rules across context consumer modules.

**Accept when:**

- All context consumer components unwrapping promises compile and pass static verification without hook rule violations.
- Component tests verify successful data unwrapping and proper fallback handling during pending promise states.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated continuous integration pipeline running static analysis and test validation suites.
- Verified by: Peer code review verifying adherence to context promise unwrapping patterns.
- Violation handling: Pull request checks block merging if unapproved asynchronous context consumption patterns are detected.
- Violation handling: Violations identified during code review require refactoring to use standardized context unwrapping before approval.
- Exception process: Submit an architecture exception request detailing why standard promise unwrapping via context cannot be applied.
- Exception process: Obtain formal sign-off from the technical lead before merging non-standard context implementations.
</enforcement>
