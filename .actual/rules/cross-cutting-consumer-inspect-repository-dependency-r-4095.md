---
version: 2
name: cross-cutting-consumer-inspect-repository-dependency-r-4095
description: "Adoption of lib/shopify Module for Commerce Data Integration: Consumer Inspect Repository Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: 4095669f-d422-4625-875b-8165ca34dd4a
    status: accepted
    candidate_group_id: aaff154b-2b92-4573-a820-1b4afa17030f
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/**
---

# Adoption of lib/shopify Module for Commerce Data Integration: Consumer Inspect Repository Dependency Resolution Artifact

Scope: applies to `app/**`.

These rules are ALWAYS ACTIVE for application route handlers, dynamic page renderers, and sitemap generation modules requiring commerce catalog and page data.

### Rules

- **R-SHOPIFY-001** MUST: The consumer MUST inspect the repository dependency resolution artifact to determine and verify the exact locked versions of all underlying runtime and transport dependencies before invoking module interfaces.

### Verify

```bash
# Discover and execute the project static analysis and automated test suites
# (Commands to be discovered from repository configuration)
```

**Accept when:**
- Static import analysis confirms zero direct third-party commerce network calls outside the designated module boundary.
- Automated integration verification suites pass against all routes consuming the centralized commerce module.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verified by automated static analysis checks and mandatory peer review.
</enforcement>