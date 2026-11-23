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

These rules are ALWAYS ACTIVE for:
- User interface layout and presentation components that render internal application links, navigation menus, product listings, search filters, and carousels.
- Components operating within both server and client rendering boundaries requiring client-side page transitions.

### Rules

- **R-LM-001** MUST_NOT: Components MUST NOT use next/link for external outbound destinations targeting third-party domains, which must use standard hyperlink elements with appropriate security attributes.

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
