# Use Console Error Logging for Invalid Revalidation Secrets in Internal API Handlers: External Service Client

These rules are ALWAYS ACTIVE for internal API handlers that validate webhook secrets, request processing functions that extract secrets from query parameters, and security validation logic that compares request secrets against environment configuration.

### Rules

- **R-CONSOLE-001** SHOULD: External service client functions SHOULD use async patterns and accept custom headers for authentication.
- **R-CONSOLE-002** MUST: Console error logging MUST be used to record security validation failures for invalid or missing revalidation secrets without introducing dependencies on structured logging frameworks.
- **R-CONSOLE-003** MUST: Error messages MUST clearly indicate validation failure without including the expected or received secret values, tokens, or sensitive configuration in console output.
- **R-CONSOLE-004** SHOULD: Console error calls SHOULD be wrapped in a utility function that enforces consistent error message formatting and prevents accidental secret exposure.
- **R-CONSOLE-005** MUST: All secret validation failure paths MUST produce appropriate console error logging for visibility during development and production debugging.

### Verify

```bash
# Discover the project's test suite location and execute tests that validate internal API handlers reject requests with invalid or missing revalidation secrets
find . -type f -name "*.test.*" -o -name "*.spec.*" | grep -i "handler\|webhook\|secret" | head -5

# Locate and run the project's security validation test cases to confirm console error output appears for authentication failures
grep -r "console\.error" --include="*.test.*" --include="*.spec.*" | grep -i "secret\|validation\|auth" | head -10

# Identify the project's integration test configuration and verify webhook handler behavior with invalid secrets produces expected console error messages
grep -r "invalid.*secret\|missing.*secret" --include="*.test.*" --include="*.spec.*" --include="*.js" --include="*.ts" | head -10

# Verify no secret values are exposed in console error statements
grep -r "console\.error" --include="*.js" --include="*.ts" | grep -v "test" | grep -i "secret\|token\|auth" | head -20
```

**Accept when:**
- Internal API handlers reject requests with invalid revalidation secrets and produce console error output describing the validation failure
- Console error messages do not expose actual secret values, tokens, or sensitive configuration in their output
- Test coverage confirms that all secret validation failure paths produce appropriate console error logging
- Error messages are formatted consistently through a utility function or wrapper
- External service client functions use async patterns and accept custom headers for authentication

<enforcement>
Clause Code MUST NOT skip or defer verification. All rules in this file are mandatory for pull requests affecting internal API handlers, webhook validation, or secret authentication logic. Code review MUST confirm console error statements do not expose secret values. Automated test suite execution MUST confirm error logging behavior for invalid secrets. Security audit MUST verify validation failure visibility. Pull requests that add secret validation without console error logging MUST be revised before merge. Code that exposes secret values in console output MUST be immediately patched.
</enforcement>