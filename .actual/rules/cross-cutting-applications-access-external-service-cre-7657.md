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

These rules are ALWAYS ACTIVE for:
- All server-side data fetching adapters and external API integration clients.
- Webhook request handlers and cache revalidation endpoints.
- UI components and metadata generators consuming runtime environment configurations.

### Rules

- **R-SC-001** MUST: Applications MUST access external service credentials, webhook verification secrets, and runtime environment settings through the Next.js process environment abstraction rather than hardcoded literals.

### Verify

- Discover the repository static analysis script and execute it to verify that environment variables are accessed strictly through approved patterns.
- Discover the repository build script and run it to verify that no server-only secrets are leaked into client bundle artifacts.
- Discover the repository test runner from the dependency manifest and run the suite to validate secret comparison and revalidation error handling.

**Accept when:**

- Static analysis confirms no sensitive environment variables are imported or referenced within client-side component trees.
- The project build succeeds without emitting warnings regarding credential leakage into browser bundles.
- Automated tests pass validating that external client requests include required credential headers and that invalid webhook secrets are rejected.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Automated CI pipeline static analysis scanning for forbidden environment variable leaks into client bundles.
- Verified by: Pull request code reviews auditing new external client integrations and webhook handlers.
- Violation handling: CI pipeline failure blocking merge when server-only environment variables are referenced in client bundles.
- Violation handling: Immediate revocation and rotation of any credentials discovered in committed source code.
- Exception process: Exceptions for non-sensitive public configuration require review and sign-off by the application architecture and security leads.
</enforcement>
