# Isolate Environment-Sourced Secrets in Async Function Boundaries: Async Functions That

These rules are ALWAYS ACTIVE for all async functions that access environment-sourced configuration or secrets within async function boundaries that coordinate external API calls and server-side rendering operations.

### Rules

- **R-ASYNC-SEC-001** SHOULD: Async functions that use secrets for external authentication SHOULD fail fast with non-revealing error messages when secrets are missing or invalid.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules
# that detect direct environment variable access patterns in async function bodies.
find . -name '.eslintrc*' -o -name 'eslint.config.*' -o -name '.pylintrc' -o -name 'pyproject.toml' | head -5

# Locate the project's test suite and run integration tests that verify secret
# validation behavior, including tests for missing secrets, invalid formats, and
# proper error message redaction.
find . -type f \( -name '*.test.*' -o -name '*.spec.*' -o -name 'test_*.py' \) | grep -i secret | head -10

# Identify the project's security scanning tools and execute checks that detect
# potential secret leakage in logging statements, error messages, and serialized responses.
find . -name '.trivy.yaml' -o -name 'semgrep.yml' -o -name '.bandit' -o -name 'safety.json' | head -5

# Search for direct process.env or os.environ access in async function bodies
grep -r "process\.env\|os\.environ" --include="*.js" --include="*.ts" --include="*.py" . 2>/dev/null | grep -i "async\|await" | head -20
```

**Accept when:**
- All async functions that use environment-sourced secrets retrieve them through a centralized validation module rather than direct environment access.
- Static analysis or linting passes without detecting direct process environment access in async function bodies outside approved patterns.
- Integration tests confirm that missing or invalid secrets produce non-revealing error messages and that secret values never appear in logs or error responses.
- Security scanning tools report no instances of potential secret leakage in observability outputs or client-visible responses.

<enforcement>
Claude Code MUST NOT skip or defer verification. All four acceptance criteria MUST be confirmed before marking this rule as satisfied.
</enforcement>