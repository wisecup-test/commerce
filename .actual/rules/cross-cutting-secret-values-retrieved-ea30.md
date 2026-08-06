# Isolate Environment-Sourced Secrets in Async Function Boundaries: Secret Values Retrieved

These rules are ALWAYS ACTIVE for all async functions that retrieve configuration or secrets from process environment variables, functions that construct HTTP headers or request bodies containing authentication credentials, server-side rendering components that access environment-sourced configuration, and API integration modules that coordinate external service calls using secrets.

### Rules

- **R-SECRET-001** MUST: Secret values retrieved from environment sources MUST NOT be logged, serialized to client-visible responses, or included in error messages that may be exposed beyond server boundaries.
- **R-SECRET-002** MUST: All async functions that use environment-sourced secrets retrieve them through a centralized validation module rather than direct environment access.
- **R-SECRET-003** MUST: Create a centralized secret validation module that exports typed functions for retrieving and validating each required secret, serving as the single source of truth for environment variable names and validation rules.
- **R-SECRET-004** MUST: Implement a request-scoped context or container that holds validated secrets for the duration of a single request lifecycle to prevent repeated environment access and ensure secrets do not leak across request boundaries.
- **R-SECRET-005** SHOULD: Add linting rules or static analysis checks to detect direct process environment access in async functions outside the approved validation module.
- **R-SECRET-006** SHOULD: Document the classification of each environment variable as either secret or non-secret configuration, with explicit review of classifications.
- **R-SECRET-007** MAY: Non-secret environment variables such as feature flags or public site names may be accessed directly without validation isolation (EXC-001).
- **R-SECRET-008** MAY: Webhook validation endpoints may log secret comparison failures for security monitoring purposes, provided the actual secret value is never included in logs (EXC-002).

### Verify

```bash
# Discover the project's static analysis configuration and execute linting rules
# that detect direct environment variable access patterns in async function bodies
find . -name ".eslintrc*" -o -name "eslint.config.*" -o -name "tsconfig.json" | head -1

# Locate the project's test suite and run integration tests
# that verify secret validation behavior
find . -type f \( -name "*.test.ts" -o -name "*.test.js" -o -name "*.spec.ts" -o -name "*.spec.js" \) | grep -i secret | head -5

# Identify the project's security scanning tools and execute checks
# that detect potential secret leakage in logging statements
find . -name "package.json" -o -name "pyproject.toml" -o -name "Gemfile" | xargs grep -l "snyk\|trivy\|semgrep" 2>/dev/null || echo "No security scanner found in manifest"

# Search for direct process.env access in async functions
grep -r "process\.env" --include="*.ts" --include="*.js" | grep -E "async|await" | head -10

# Verify centralized secret validation module exists
find . -type f \( -name "*secret*" -o -name "*config*" \) \( -name "*.ts" -o -name "*.js" \) | grep -v node_modules | grep -v dist | head -5
```

**Accept when:**
- All async functions that use environment-sourced secrets retrieve them through the centralized validation module rather than direct environment access.
- Static analysis or linting passes without detecting direct process environment access in async function bodies outside approved patterns.
- Integration tests confirm that missing or invalid secrets produce non-revealing error messages and that secret values never appear in logs or error responses.
- Security scanning tools report no instances of potential secret leakage in observability outputs or client-visible responses.
- Request-scoped context or container implementation prevents secret leakage across request boundaries.
- Environment variable classifications (secret vs. non-secret) are documented and reviewed.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before accepting code changes. Static analysis, integration tests, and security scanning must pass without exceptions unless explicitly approved through the documented exception process.
</enforcement>