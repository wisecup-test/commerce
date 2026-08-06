# Isolate Environment-Sourced Secrets in Async Function Boundaries: Async Functions That

These rules are ALWAYS ACTIVE for all async functions that access environment-sourced configuration or secrets within async function boundaries that coordinate external API calls and server-side rendering operations.

### Rules

- **R-ASYNC-SEC-001** MUST: All async functions that access environment-sourced secrets MUST validate secret presence and format before using them in external calls or header construction.
- **R-ASYNC-SEC-002** MUST: Create a centralized secret validation module that exports typed functions for retrieving and validating each required secret. This module MUST be the single source of truth for environment variable names and validation rules.
- **R-ASYNC-SEC-003** MUST: Implement a request-scoped context or container that holds validated secrets for the duration of a single request lifecycle to prevent repeated environment access while ensuring secrets do not leak across request boundaries.
- **R-ASYNC-SEC-004** MUST: Add linting rules or static analysis checks to detect direct process environment access in async functions outside the approved validation module.
- **R-ASYNC-SEC-005** MUST: Document the classification of each environment variable as either secret or non-secret configuration. Non-secret values may be accessed directly, but the classification MUST be explicit and reviewed.
- **R-ASYNC-SEC-006** MUST: Ensure that missing or invalid secrets produce non-revealing error messages and that secret values never appear in logs or error responses.
- **R-ASYNC-SEC-007** MAY: Non-secret environment variables such as feature flags or public site names may be accessed directly without validation isolation (EXC-001).
- **R-ASYNC-SEC-008** MAY: Webhook validation endpoints may log secret comparison failures for security monitoring purposes, provided the actual secret value is never included in logs (EXC-002).

### Verify

```bash
# Discover the project's static analysis configuration and execute linting rules
# that detect direct environment variable access patterns in async function bodies
echo "Executing static analysis for direct environment variable access in async functions..."

# Locate the project's test suite and run integration tests that verify secret validation
echo "Running integration tests for secret validation behavior..."

# Identify the project's security scanning tools and execute checks for secret leakage
echo "Executing security scanning for potential secret leakage in logs and error messages..."
```

**Accept when:**
- All async functions that use environment-sourced secrets retrieve them through the centralized validation module rather than direct environment access.
- Static analysis or linting passes without detecting direct process environment access in async function bodies outside approved patterns.
- Integration tests confirm that missing or invalid secrets produce non-revealing error messages and that secret values never appear in logs or error responses.
- Security scanning tools report no instances of potential secret leakage in observability outputs or client-visible responses.

<enforcement>
Claude Code MUST NOT skip or defer verification. All async functions accessing environment-sourced secrets MUST be validated against these rules before code is committed. Violations discovered in production code are treated as security incidents requiring hotfix deployment.
</enforcement>