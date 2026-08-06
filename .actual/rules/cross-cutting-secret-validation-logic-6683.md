# Isolate Environment-Sourced Secrets in Async Function Boundaries: Secret Validation Logic

These rules are ALWAYS ACTIVE for all async functions that retrieve configuration or secrets from process environment variables, functions that construct HTTP headers or request bodies containing authentication credentials, server-side rendering components that access environment-sourced configuration, and API integration modules that coordinate external service calls using secrets.

### Rules

- **R-SECRET-001** SHOULD: Secret validation logic SHOULD be centralized in a dedicated module to ensure consistent error handling and audit trail generation across all async function boundaries.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that detect direct environment variable access patterns in async function bodies.
# Locate the project's test suite and run integration tests that verify secret validation behavior, including tests for missing secrets, invalid formats, and proper error message redaction.
# Identify the project's security scanning tools and execute checks that detect potential secret leakage in logging statements, error messages, and serialized responses.
```

**Accept when:**
- All async functions that use environment-sourced secrets retrieve them through the centralized validation module rather than direct environment access.
- Static analysis or linting passes without detecting direct process environment access in async function bodies outside approved patterns.
- Integration tests confirm that missing or invalid secrets produce non-revealing error messages and that secret values never appear in logs or error responses.
- Security scanning tools report no instances of potential secret leakage in observability outputs or client-visible responses.

<enforcement>
Claude Code MUST NOT skip or defer verification. All async functions accessing environment-sourced secrets MUST route through centralized validation. Direct environment variable access in async function bodies outside the approved validation module is a violation. Code review and static analysis MUST block violations. Security scanning alerts MUST trigger immediate review.
</enforcement>