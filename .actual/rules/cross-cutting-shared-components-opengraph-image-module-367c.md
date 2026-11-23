---
version: 2
name: cross-cutting-shared-components-opengraph-image-module-367c
description: "Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Shared Components Opengraph Image Module Maintain"
metadata:
  actual:
    adr_id: 367c796f-4cea-423f-b42c-7b48a4734dd8
    status: accepted
    candidate_group_id: f7137deb-eaa3-44c3-b6f2-14b4fdfa2721
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of components/opengraph-image for Centralized OpenGraph Image Generation: Shared Components Opengraph Image Module Maintain

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for:
- All route-level social metadata and OpenGraph image generation handlers across root and dynamic route segments.

### Rules

- **R-LM-001** MUST: The shared `components/opengraph-image` module MUST maintain a stable presentation interface accepting parameterized title and context inputs across consuming route handlers.

### Verify

- Discover the repository build and static analysis commands from package manifests and run lint checks across all route image handlers.
- Discover and execute the project test runner to verify that social image generation handlers resolve and execute without errors.

**Accept when:**

- Static analysis and lint checks complete with zero errors across all OpenGraph image route handlers.
- All OpenGraph image route endpoints successfully resolve and render preview images using the shared presentation module without interface or type mismatches.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated static analysis and code review checks during continuous integration.
- Verified by: Component integration tests verifying social image route rendering contracts.
- Violation handling: Pull requests introducing decentralized image layout markup or bypassing the shared presentation module fail automated review gates.
- Violation handling: Violations must be refactored to consume the shared image component before branch merge.
- Exception process: Submit an architectural review request documenting why the shared image component cannot satisfy the visual or technical requirements of the route handler.
- Exception process: Obtain approval from frontend engineering leads before introducing bespoke image generation logic.
</enforcement>
