---
version: 2
name: cross-cutting-applications-access-external-service-cre-7657
description: "Next.js Secrets Management and Runtime Environment Access: Applications Access External Service Credentials Webhook"
metadata:
  actual:
    adr_id: 76579da8-ebbb-4f19-8809-7ee801697af8
    status: accepted
    candidate_group_id: 6554969c-07a0-4c49-9b32-231648e13714
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - components/**
      - lib/**
---

# Next.js Secrets Management and Runtime Environment Access: Applications Access External Service Credentials Webhook

Scope: applies to `components/**`, `lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SEC-001** MUST: Applications MUST access external service credentials, webhook verification secrets, and runtime environment settings through the Next.js process environment abstraction rather than hardcoded literals.

### Verify

```bash
# Discover and run the repository static analysis script to verify environment variable usage patterns
npm run lint

# Discover and run the repository build script to verify no server-only secrets leak into client bundles
npm run build

# Discover and run the test suite to validate secret comparison and revalidation error handling
npm test
```

**Accept when:**
- Static analysis confirms no sensitive environment variables are imported or referenced within client-side component trees.
- The project build succeeds without emitting warnings regarding credential leakage into browser bundles.
- Automated tests pass validating that external client requests include required credential headers and that invalid webhook secrets are rejected.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated CI pipeline static analysis and pull request code reviews enforce these requirements, and violations result in CI pipeline failure and immediate credential rotation.
</enforcement>