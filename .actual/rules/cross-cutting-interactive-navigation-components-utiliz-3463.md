---
version: 2
name: cross-cutting-interactive-navigation-components-utiliz-3463
description: "Adoption of next/link for Declarative Client-Side Navigation: Interactive Navigation Components Utilizing Client Side"
metadata:
  actual:
    adr_id: 34635096-549e-4c71-8e94-ad2fb4506bbe
    status: accepted
    candidate_group_id: 2ab43c5c-4665-4c51-bd45-48eecd759009
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of next/link for Declarative Client-Side Navigation: Interactive Navigation Components Utilizing Client Side

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** SHOULD: Interactive navigation components utilizing client-side hooks alongside next/link SHOULD isolate client interaction boundaries using explicit client rendering directives while keeping non-interactive presentation shells as server-rendered components where applicable.

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