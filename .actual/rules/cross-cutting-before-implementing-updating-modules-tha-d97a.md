---
version: 2
name: cross-cutting-before-implementing-updating-modules-tha-d97a
description: "Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Before Implementing Updating Modules That Rely"
metadata:
  actual:
    adr_id: d97acd07-0836-4717-a2e4-5ce63e600224
    status: accepted
    candidate_group_id: f7137deb-eaa3-44c3-b6f2-14b4fdfa2721
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Before Implementing Updating Modules That Rely

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OG-001** MUST: Before implementing or updating modules that rely on versioned rendering or graphic generation dependencies, developers MUST inspect the repository lock artifact and verify exact resolved dependency versions against current documentation.

### Verify

```bash
# Discover repository build, static analysis, and test commands from manifests and execute lint checks and tests across all route image handlers.
```

**Accept when:**
- Static analysis and lint checks complete with zero errors across all OpenGraph image route handlers.
- All OpenGraph image route endpoints successfully resolve and render preview images using the shared presentation module without interface or type mismatches.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis, code review checks, and component integration tests during continuous integration.
</enforcement>