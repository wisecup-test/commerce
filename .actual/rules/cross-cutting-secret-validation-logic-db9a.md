# Use Console Error Logging for Invalid Revalidation Secrets in Internal API Handlers: Secret Validation Logic

These rules are ALWAYS ACTIVE for internal API route handlers that validate webhook secrets, request processing functions that extract secrets from query parameters, and security validation logic that compares request secrets against environment configuration.

### Rules

- **R-SECRET-001** MUST: Secret validation logic MUST extract comparison values from runtime environment configuration.
- **R-SECRET-002** MUST: Console error messages MUST NOT expose actual secret values, tokens, or sensitive configuration in their output.
- **R-SECRET-003** MUST: All secret validation failure paths MUST produce appropriate console error logging.
- **R-SECRET-004** SHOULD: Wrap console error calls in a utility function that enforces consistent error message formatting and prevents accidental secret exposure.
- **R-SECRET-005** SHOULD: Error messages MUST clearly indicate validation failure without including the expected or received secret values in the console output.

### Verify

```bash
# Discover the project's test suite location and execute tests that validate internal API handlers reject requests with invalid or missing revalidation secrets
find . -type f -name "*.test.*" -o -name "*.spec.*" | grep -i "secret\|validation\|webhook" | head -5

# Locate and run the project's security validation test cases to confirm console error output appears for authentication failures
grep -r "console\.error" --include="*.test.*" --include="*.spec.*" | grep -i "secret\|auth\|validation" | head -10

# Identify the project's integration test configuration and verify webhook handler behavior with invalid secrets produces expected console error messages
grep -r "invalid.*secret\|missing.*secret" --include="*.test.*" --include="*.spec.*" --include="*.js" --include="*.ts" | head -10

# Verify no secret values are exposed in console error statements
grep -r "console\.error" --include="*.js" --include="*.ts" | grep -v "test" | grep -i "secret\|auth" | head -10
```

**Accept when:**
- Internal API handlers reject requests with invalid revalidation secrets and produce console error output describing the validation failure
- Console error messages do not expose actual secret values, tokens, or sensitive configuration in their output
- Test coverage confirms that all secret validation failure paths produce appropriate console error logging
- Secret validation logic extracts comparison values from runtime environment configuration
- Code review verification confirms console error statements do not expose secret values

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for pull requests affecting internal API handlers that validate webhook secrets. Violations must be corrected before merge.
</enforcement>