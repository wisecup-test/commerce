# Isolate Environment-Sourced Secrets in Async Function Boundaries: System Cache Validated

These rules are ALWAYS ACTIVE for all async functions that retrieve configuration or secrets from process environment variables, functions that construct HTTP headers or request bodies containing authentication credentials, server-side rendering components that access environment-sourced configuration, and API integration modules that coordinate external service calls using secrets.

### Rules

- **R-SEC-001** MAY: The system MAY cache validated secret references within a single request context to avoid repeated environment access, provided the cache is scoped to the request lifecycle.
- **R-SEC-002** MUST: All async functions that retrieve environment-sourced secrets MUST retrieve them through a centralized validation module rather than direct environment access.
- **R-SEC-003** MUST: Secret values MUST never appear in logs, error messages, or serialized responses.
- **R-SEC-004** MUST: All error logging statements in functions that access environment-sourced secrets MUST implement secret redaction.
- **R-SEC-005** SHOULD: Developers SHOULD classify each environment variable explicitly as either secret or non-secret configuration.
- **R-SEC-006** MAY: Non-secret environment variables such as feature flags or public site names MAY be accessed directly without validation isolation (EXC-001).
- **R-SEC-007** MAY: Webhook validation endpoints MAY log secret comparison failures for security monitoring purposes, provided the actual secret value is never included in logs (EXC-002).

### Verify

```bash
# Discover the project's static analysis configuration and execute linting rules
# that detect direct environment variable access patterns in async function bodies.
echo "Executing static analysis for direct environment access in async functions..."

# Locate the project's test suite and run integration tests that verify secret
# validation behavior, including tests for missing secrets, invalid formats,
# and proper error message redaction.
echo "Running integration tests for secret validation behavior..."

# Identify the project's security scanning tools and execute checks that detect
# potential secret leakage in logging statements, error messages, and serialized responses.
echo "Executing security scanning for potential secret leakage..."
```

**Accept when:**
- All async functions that use environment-sourced secrets retrieve them through the centralized validation module rather than direct environment access.
- Static analysis or linting passes without detecting direct process environment access in async function bodies outside approved patterns.
- Integration tests confirm that missing or invalid secrets produce non-revealing error messages and that secret values never appear in logs or error responses.
- Security scanning tools report no instances of potential secret leakage in observability outputs or client-visible responses.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations discovered in production code are treated as security incidents requiring hotfix deployment. Code review blocks merge until secret handling follows approved isolation patterns. Continuous integration pipeline fails if static analysis detects unapproved direct environment access patterns.
</enforcement>