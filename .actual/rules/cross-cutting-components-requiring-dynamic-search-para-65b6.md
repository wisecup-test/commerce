---
version: 2
name: cross-cutting-components-requiring-dynamic-search-para-65b6
description: "Adoption of next/link for Declarative Client-Side Navigation: Components Requiring Dynamic Search Parameters Query"
metadata:
  actual:
    adr_id: 65b6f013-e7d7-4654-9fb7-17281175a522
    status: accepted
    candidate_group_id: 2ab43c5c-4665-4c51-bd45-48eecd759009
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
---

# Adoption of next/link for Declarative Client-Side Navigation: Components Requiring Dynamic Search Parameters Query

Scope: applies to `components/**`.

These rules are ALWAYS ACTIVE for:
- User interface layout and presentation components that render internal application links, navigation menus, product listings, search filters, and carousels.
- Components operating within both server and client rendering boundaries requiring client-side page transitions.

### Rules

- **R-LM-001** SHOULD: Components requiring dynamic search parameters or query string preservation SHOULD compute the target route URL using URLSearchParams or query manipulation abstractions before passing the resulting path string to next/link.

### Verify

- Discover and run the project static analysis and linting validation scripts to confirm all internal navigational links utilize next/link.
- Discover and run the project automated test suite to ensure navigational components render valid destination route attributes without runtime errors.

**Accept when:**

- All internal navigation elements across layout and presentation components resolve to next/link instances without unmanaged internal anchor elements.
- Static analysis and test suites execute with zero navigation-related lint errors or broken route reference failures.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated continuous integration lint checks enforcing import rules for internal navigation.
- Verified by: Peer code review for all pull requests introducing new layout or presentation link components.
- Violation handling: Pull requests containing native anchor tags for internal application navigation are blocked until refactored to use next/link.
- Exception process: Developers submit an architecture exception request detailing why standard client-side routing is incompatible with the specific link destination.
</enforcement>
