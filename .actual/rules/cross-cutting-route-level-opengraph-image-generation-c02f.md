---
version: 2
name: cross-cutting-route-level-opengraph-image-generation-c02f
description: "Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Route Level Opengraph Image Generation Endpoints"
metadata:
  actual:
    adr_id: c02fd1fd-a5a5-4d4b-94a5-0b34b3ec6012
    status: accepted
    candidate_group_id: f7137deb-eaa3-44c3-b6f2-14b4fdfa2721
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Route Level Opengraph Image Generation Endpoints

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OG-001** MUST: All route-level OpenGraph image generation endpoints MUST delegate visual presentation and image card layout to the shared `components/opengraph-image` module.

### Verify

```bash
# Discover the repository build and static analysis commands from package manifests and run lint checks across all route image handlers.
# Discover and execute the project test runner to verify that social image generation handlers resolve and execute without errors.
```

**Accept when:**
- Static analysis and lint checks complete with zero errors across all OpenGraph image route handlers.
- All OpenGraph image route endpoints successfully resolve and render preview images using the shared presentation module without interface or type mismatches.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis, code review checks, and component integration tests must verify compliance.
</enforcement>