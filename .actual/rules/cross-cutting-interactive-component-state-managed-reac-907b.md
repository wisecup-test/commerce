---
version: 2
name: cross-cutting-interactive-component-state-managed-reac-907b
description: "React Client Component State and Effect Hooks Pattern: Interactive Component State Managed React Hook"
metadata:
  actual:
    adr_id: 907b8ebf-b4f6-4a78-87e7-84e262fa6954
    status: accepted
    candidate_group_id: 47fe226a-46d7-4ea3-8c60-35676ab361f8
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# React Client Component State and Effect Hooks Pattern: Interactive Component State Managed React Hook

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for:
- Interactive UI components handling user input, modal visibility, menu state, and navigation filter interactions.
- Client context providers coordinating shared client-side application state.

### Rules

- **R-IP-001** MUST: Interactive component state MUST be managed using React hook APIs, utilizing useState for discrete interaction state transitions and useEffect for browser or navigation parameter synchronization.

### Verify

- Discover the test script in the project manifest and execute the test runner to verify interactive component behavior.
- Discover the linting script in the project manifest and execute the static analyzer to check client directive and hook usage.
- Discover the build verification script in the project manifest and execute the compiler to confirm type safety and client boundary exports.

**Accept when:**

- Static analysis validates that all components consuming interactive hooks declare the client component directive.
- Unit and component tests verify that state transitions and effect hooks function correctly across user interactions.
- The build compilation step completes without type errors or server-client boundary violations.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis checks in the continuous integration pipeline validating hook usage and directive presence.
- Verified by: Peer code reviews verifying appropriate separation of client interaction boundaries and state management.
- Violation handling: Pull requests containing interactive hooks in non-client components fail automated continuous integration checks.
- Violation handling: Code review comments mandate refactoring when interactive state logic violates component boundary guidelines.
- Exception process: Architectural exceptions require written documentation and approval from the frontend technical lead.
</enforcement>
