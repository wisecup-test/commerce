---
version: 2
name: cross-cutting-sensitive-external-service-credentials-v-708e
description: "Next.js Secrets Management and Runtime Environment Access: Sensitive External Service Credentials Verification Tokens"
metadata:
  actual:
    adr_id: 708e98b7-0400-41c5-808b-35ae98a11810
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

# Next.js Secrets Management and Runtime Environment Access: Sensitive External Service Credentials Verification Tokens

Scope: applies to `components/**`, `lib/**`.

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-SEC-001** MUST_NOT: Sensitive external service credentials and verification tokens MUST NOT be exposed to client-accessible bundles, public environment prefixes, or browser runtime contexts.

### Verify

```bash
# Discover and run the repository static analysis script to verify environment variables usage
# Discover and run the repository build script to verify no server-only secrets leak into client bundles
# Discover and run the repository test runner to validate secret comparison and revalidation error handling
```

**Accept when:**
- Static analysis confirms no sensitive environment variables are imported or referenced within client-side component trees.
- The project build succeeds without emitting warnings regarding credential leakage into browser bundles.
- Automated tests pass validating that external client requests include required credential headers and that invalid webhook secrets are rejected.

<enforcement>
Claude Code MUST NOT skip or defer verification. Verification is mandatory.
</enforcement>