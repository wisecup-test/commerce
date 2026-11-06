---
version: 2
name: cross-cutting-webhook-endpoints-revalidation-triggers-2e6a
description: "Next.js Secrets Management and Runtime Environment Access: Webhook Endpoints Revalidation Triggers Validate Incoming"
metadata:
  actual:
    adr_id: 2e6a785c-0f1f-4748-822e-8adbc2c0696c
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

# Next.js Secrets Management and Runtime Environment Access: Webhook Endpoints Revalidation Triggers Validate Incoming

Scope: applies to `components/**`, `lib/**`.

These rules are ALWAYS ACTIVE for all webhook endpoints, revalidation triggers, and server-side data fetching adapters.

### Rules

- **R-SEC-001** MUST: Webhook endpoints and revalidation triggers MUST validate incoming request secrets against the configured process environment secret and MUST record structured error logs upon validation failure.

### Verify

```bash
# Discover and execute repository static analysis script
npm run lint || yarn lint || pnpm lint
# Discover and execute repository build script
npm run build || yarn build || pnpm build
# Discover and execute test runner
npm test || yarn test || pnpm test
```

**Accept when:**
- Static analysis confirms no sensitive environment variables are imported or referenced within client-side component trees.
- The project build succeeds without emitting warnings regarding credential leakage into browser bundles.
- Automated tests pass validating that external client requests include required credential headers and that invalid webhook secrets are rejected.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>