---
version: 2
name: cross-cutting-components-not-use-next-link-a078
description: "Adoption of next/link for Declarative Client-Side Navigation: Components Not Use Next Link External"
metadata:
  actual:
    adr_id: a078bde1-d43f-4b43-a640-e4f0d3a165ba
    status: accepted
    candidate_group_id: 2ab43c5c-4665-4c51-bd45-48eecd759009
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of next/link for Declarative Client-Side Navigation: Components Not Use Next Link External

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** MUST_NOT: Components MUST NOT use next/link for external outbound destinations targeting third-party domains, which must use standard hyperlink elements with appropriate security attributes.

### Verify

```bash
# Discover and run project static analysis, linting scripts, and automated test suite
npm run lint
npm test
```

**Accept when:**
- All internal navigation elements across layout and presentation components resolve to next/link instances without unmanaged internal anchor elements.
- Static analysis and test suites execute with zero navigation-related lint errors or broken route reference failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>