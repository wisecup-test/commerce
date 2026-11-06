---
version: 2
name: cross-cutting-route-handlers-handle-asynchronous-data-50bb
description: "Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Route Handlers Handle Asynchronous Data Resolution"
metadata:
  actual:
    adr_id: 50bb0b08-2cb0-4c85-a559-8095abd310aa
    status: accepted
    candidate_group_id: f7137deb-eaa3-44c3-b6f2-14b4fdfa2721
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Route Handlers Handle Asynchronous Data Resolution

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-OG-001** SHOULD: Route handlers SHOULD handle all asynchronous data resolution and fallback parameter formatting prior to passing title and description properties into the image generation component.

### Verify

```bash
# Discover the repository build and static analysis commands from package manifests and run lint checks across all route image handlers.
# Discover and execute the project test runner to verify that social image generation handlers resolve and execute without errors.
```

**Accept when:**
- Static analysis and lint checks complete with zero errors across all OpenGraph image route handlers.
- All OpenGraph image route endpoints successfully resolve and render preview images using the shared presentation module without interface or type mismatches.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violation handling: Pull requests introducing decentralized image layout markup or bypassing the shared presentation module fail automated review gates.
</enforcement>