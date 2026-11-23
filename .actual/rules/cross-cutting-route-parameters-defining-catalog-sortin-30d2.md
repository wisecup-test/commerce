---
version: 2
name: cross-cutting-route-parameters-defining-catalog-sortin-30d2
description: "Shopify Client Module (lib/shopify): Route Parameters Defining Catalog Sorting Criteria"
metadata:
  actual:
    adr_id: 30d22d5a-ebf8-4828-a0ed-f837f86fb725
    status: accepted
    candidate_group_id: a61abcae-9d2e-4506-ac32-4166f16a7b12
    evidence:
      source: deep
  scope:
    root: .
    paths:
      - app/search/**
---

# Shopify Client Module (lib/shopify): Route Parameters Defining Catalog Sorting Criteria

Scope: applies to `app/search/**`.

These rules are ALWAYS ACTIVE for:
- Route handlers, page components, and search views requiring e-commerce product and collection data.
- Internal modules managing catalog queries, collection filtering, and product sorting.

### Rules

- **R-LM-001** SHOULD: Route parameters defining catalog sorting criteria SHOULD be validated against established sorting constants before invoking data queries on the integration module.

### Verify

- Inspect the repository configuration to locate and execute the project's static analysis script to verify module import boundary compliance.
- Discover and run the project's automated test runner from the root manifest to validate catalog retrieval and sorting query behavior.

**Accept when:**

- All automated test suites covering catalog data fetching and search query resolution pass without regressions.
- Static analysis and module boundary checks confirm no direct external store API requests bypass the centralized module.

<enforcement>
Claude Code MUST NOT skip or defer verification.
- Verified by: Continuous integration static analysis rules checking import boundaries to prevent unauthorized external network requests in presentation layers.
- Verified by: Automated integration test suites validating module contracts against resolved dependency versions.
- Verified by: Peer code review verifying adherence to centralized data client module patterns.
- Violation handling: Static analysis failures immediately block pull request merges when presentation components bypass the integration module.
- Violation handling: Violations flagged during review require refactoring unauthorized external requests into the designated client module.
- Exception process: Submit an architectural review request detailing external store capabilities that cannot be accommodated within the centralized module boundary.
- Exception process: Document justification and obtain approval from the core engineering team prior to deploying isolated network calls.
</enforcement>
