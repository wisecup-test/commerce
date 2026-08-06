# Use Console Error Logging for Invalid Revalidation Secrets in Internal API Handlers: Internal Handlers Log

These rules are ALWAYS ACTIVE for all internal API handlers that validate webhook secrets by extracting secret parameters from request query strings and comparing them against environment configuration.

### Rules

- **R-INTERNAL-001** MUST: Internal API handlers MUST log invalid revalidation secret attempts using console error output.
- **R-INTERNAL-002** MUST: Console error messages MUST NOT expose actual secret values, tokens, or sensitive configuration in their output.
- **R-INTERNAL-003** MUST: All secret validation failure paths MUST produce appropriate console error logging.
- **R-INTERNAL-004** SHOULD: Consider wrapping console error calls in a utility function that enforces consistent error message formatting and prevents accidental secret exposure.

### Verify

```bash
# Discover the project's test suite location and execute tests that validate internal API handlers reject requests with invalid or missing revalidation secrets
find . -type f -name "*.test.*" -o -name "*.spec.*" | grep -i "handler\|webhook\|secret" | head -5

# Locate and run the project's security validation test cases to confirm console error output appears for authentication failures
grep -r "console\.error" --include="*.test.*" --include="*.spec.*" | grep -i "secret\|validation\|auth" | head -10

# Identify the project's integration test configuration and verify webhook handler behavior with invalid secrets produces expected console error messages
grep -r "invalid.*secret\|missing.*secret" --include="*.test.*" --include="*.spec.*" --include="*.js" --include="*.ts" | head -10

# Verify no secret values are exposed in console error statements
grep -r "console\.error" --include="*.js" --include="*.ts" | grep -v "test" | grep -i "secret\|revalidation" | head -10
```

**Accept when:**
- Internal API handlers reject requests with invalid revalidation secrets and produce console error output describing the validation failure
- Console error messages do not expose actual secret values, tokens, or sensitive configuration in their output
- Test coverage confirms that all secret validation failure paths produce appropriate console error logging
- Code review verification confirms console error statements do not expose secret values
- Automated test suite execution confirms error logging behavior for invalid secrets
- Security audit of internal API handlers verifies validation failure visibility

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests that add secret validation without console error logging must be revised before merge. Code that exposes secret values in console output must be immediately patched. Missing test coverage for validation failure logging triggers CI failure.
</enforcement>