# Isolate Environment-Sourced Secrets in Async Function Boundaries: Async Functions That

These rules are ALWAYS ACTIVE for all async functions that access environment-sourced configuration or secrets within async function boundaries that coordinate external API calls and server-side rendering operations.

### Rules

- **R-ASYNC-SEC-001** MUST: Async functions that coordinate external API calls using secrets MUST isolate secret retrieval from business logic and treat secrets as opaque values after validation.
- **R-ASYNC-SEC-002** MUST: All async functions that retrieve configuration or secrets from process environment variables MUST do so through a centralized validation module rather than direct environment access.
- **R-ASYNC-SEC-003** MUST: Secret values MUST never appear in logs, error messages, or serialized responses.
- **R-ASYNC-SEC-004** MUST: Functions that construct HTTP headers or request bodies containing authentication credentials MUST validate secrets before use and prevent their exposure in observability outputs.
- **R-ASYNC-SEC-005** SHOULD: Implement a request-scoped context or container that holds validated secrets for the duration of a single request lifecycle to prevent repeated environment access and cross-request leakage.
- **R-ASYNC-SEC-006** MAY: Non-secret environment variables such as feature flags or public site names may be accessed directly without validation isolation (EXC-001).
- **R-ASYNC-SEC-007** MAY: Webhook validation endpoints may log secret comparison failures for security monitoring purposes, provided the actual secret value is never included in logs (EXC-002).

### Verify

```bash
# Discover the project's static analysis configuration and execute linting rules
# that detect direct environment variable access patterns in async function bodies
find . -name '.eslintrc*' -o -name 'eslint.config.*' -o -name '.pylintrc' -o -name 'pyproject.toml' | head -5

# Locate the project's test suite and run integration tests that verify secret validation
find . -type f \( -name '*.test.ts' -o -name '*.test.js' -o -name '*_test.py' -o -name 'test_*.py' \) | grep -i secret | head -10

# Identify the project's security scanning tools and execute checks
find . -name '.snyk*' -o -name 'trivy.yaml' -o -name '.semgrep.yml' -o -name '.bandit' 2>/dev/null

# Search for direct process.env or os.environ access in async functions
grep -r "process\.env\|os\.environ" --include="*.ts" --include="*.js" --include="*.py" | grep -E "async|await" | head -20

# Verify centralized secret validation module exists
find . -type f \( -name '*secret*' -o -name '*config*' \) -path '*/src/*' | grep -E '\.(ts|js|py)$' | head -10
```

**Accept when:**
- All async functions that use environment-sourced secrets retrieve them through a centralized validation module rather than direct environment access.
- Static analysis or linting passes without detecting direct process environment access in async function bodies outside approved patterns.
- Integration tests confirm that missing or invalid secrets produce non-revealing error messages and that secret values never appear in logs or error responses.
- Security scanning tools report no instances of potential secret leakage in observability outputs or client-visible responses.
- Code review verification confirms that new async functions follow the approved isolation patterns.

<enforcement>
Claude Code MUST NOT skip or defer verification. All async functions accessing environment-sourced secrets MUST be audited against these rules before code is committed. Violations discovered in production code are treated as security incidents requiring immediate remediation.
</enforcement>