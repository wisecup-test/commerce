---
version: 2
name: cross-cutting-frontend-user-interface-components-that-7b31
description: "Adoption of next/link for Declarative Client-Side Navigation: Frontend User Interface Components That Provide"
metadata:
  actual:
    adr_id: 7b312d55-2c63-43c0-bc80-d938ab35318b
    status: accepted
    candidate_group_id: 2ab43c5c-4665-4c51-bd45-48eecd759009
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of next/link for Declarative Client-Side Navigation: Frontend User Interface Components That Provide

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** MUST: Frontend user interface components that provide internal application routing and navigation MUST use next/link for all internal hyperlinks rather than unmanaged anchor elements or imperative window location mutations.

### Verify

```bash
# Discover and run the project static analysis and linting validation scripts to confirm all internal navigational links utilize next/link.
# Discover and run the project automated test suite to ensure navigational components render valid destination route attributes without runtime errors.
```

**Accept when:**
- All internal navigation elements across layout and presentation components resolve to next/link instances without unmanaged internal anchor elements.
- Static analysis and test suites execute with zero navigation-related lint errors or broken route reference failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>