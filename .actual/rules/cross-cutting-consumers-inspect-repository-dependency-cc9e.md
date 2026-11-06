---
version: 2
name: cross-cutting-consumers-inspect-repository-dependency-cc9e
description: "next/navigation URL Search Parameter State Management: Consumers Inspect Repository Dependency Resolution Artifact"
metadata:
  actual:
    adr_id: cc9e10d0-2acb-4411-be28-603574c88474
    status: accepted
    candidate_group_id: 3c2ba949-bbf9-45a7-9231-0f1e5cbe1593
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
      - components/cart/**
      - components/layout/**
      - components/product/**
      - lib/shopify/**
---

# next/navigation URL Search Parameter State Management: Consumers Inspect Repository Dependency Resolution Artifact

Scope: applies to `app/search/**`, `components/cart/**`, `components/layout/**`, `components/product/**`, `lib/shopify/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** MUST: Consumers MUST inspect the repository dependency resolution artifact to determine the exact locked version of next/navigation and verify compatibility with navigation APIs before implementing navigation state bindings.

### Verify

```bash
# Discover and execute repository type check and static analysis suites
# Discover and run automated test suites covering catalog filtering, search query updates, and variant selection navigation flows
```

**Accept when:**
- Static analysis and type validation suites complete without type errors across all navigation parameter access calls
- Automated navigation test suites verify that parameter mutations update URL state and correctly re-render matching catalog views

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>