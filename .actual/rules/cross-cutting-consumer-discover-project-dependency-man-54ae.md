---
version: 2
name: cross-cutting-consumer-discover-project-dependency-man-54ae
description: "React Promise Unwrapping via use API at Context Boundaries: Consumer Discover Project Dependency Manifest Inspect"
metadata:
  actual:
    adr_id: 54aeac68-3abe-43aa-a222-e7adc4717bf8
    status: accepted
    candidate_group_id: 20960f3d-e58a-4d3f-bbd1-4049364fd937
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/cart/**
---

# React Promise Unwrapping via use API at Context Boundaries: Consumer Discover Project Dependency Manifest Inspect

Scope: applies to `components/cart/**`.

These rules are ALWAYS ACTIVE for:
- Client components and context consumer hooks consuming asynchronous context promises.
- Context providers encapsulating asynchronous resource promises for subtree consumption.

### Rules

- **R-ISP-001** MUST: The consumer MUST discover the project dependency manifest and inspect the repository lock artifact to determine the exact locked version of React before utilizing asynchronous context unwrapping APIs.

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
