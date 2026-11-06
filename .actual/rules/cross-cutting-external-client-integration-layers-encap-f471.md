---
version: 2
name: cross-cutting-external-client-integration-layers-encap-f471
description: "Next.js Secrets Management and Runtime Environment Access: External Client Integration Layers Encapsulate Credential"
metadata:
  actual:
    adr_id: f471bc9b-5dfc-4a45-8b86-c0522ad947f0
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

# Next.js Secrets Management and Runtime Environment Access: External Client Integration Layers Encapsulate Credential

Scope: applies to `components/**`, `lib/**`.

These rules are ALWAYS ACTIVE for all server-side data fetching adapters, external API integration clients, webhook request handlers, cache revalidation endpoints, UI components, and metadata generators consuming runtime environment configurations.

### Rules

- **R-SEC-001** SHOULD: External client integration layers SHOULD encapsulate credential access within dedicated server-side communication adapters that inject authorization tokens into request headers.

### Verify

```bash
# Discover and execute repository static analysis script to verify environment variable access patterns
npm run lint

# Discover and execute repository build script to verify no server-only secrets leak into client bundle artifacts
npm run build

# Discover and execute the test suite to validate secret comparison and revalidation error handling
npm test
```

**Accept when:**
- Static analysis confirms no sensitive environment variables are imported or referenced within client-side component trees.
- The project build succeeds without emitting warnings regarding credential leakage into browser bundles.
- Automated tests pass validating that external client requests include required credential headers and that invalid webhook secrets are rejected.

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated CI pipeline static analysis scanning and pull request code reviews enforce adherence.
</enforcement>