---
version: 2
name: cross-cutting-components-rendering-internal-navigation-b03c
description: "Adoption of next/link for Declarative Client-Side Navigation: Components Rendering Internal Navigation Links Next"
metadata:
  actual:
    adr_id: b03c761f-bab6-479c-85eb-4ab0cdeba1ac
    status: accepted
    candidate_group_id: 2ab43c5c-4665-4c51-bd45-48eecd759009
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of next/link for Declarative Client-Side Navigation: Components Rendering Internal Navigation Links Next

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** MUST: Components rendering internal navigation links with next/link MUST provide a valid destination route descriptor to the component href property.

### Verify

```bash
# Discover and run the project static analysis and linting validation scripts to confirm all internal navigational links utilize next/link.
npm run lint

# Discover and run the project automated test suite to ensure navigational components render valid destination route attributes without runtime errors.
npm test
```

**Accept when:**
- All internal navigation elements across layout and presentation components resolve to next/link instances without unmanaged internal anchor elements.
- Static analysis and test suites execute with zero navigation-related lint errors or broken route reference failures.

<enforcement>
Claude Code MUST NOT skip or defer verification. All internal navigation links must use next/link and pass automated lint and test checks.
</enforcement>