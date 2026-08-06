# Use Console Error Logging for Invalid Revalidation Secrets in Internal API Handlers: Error Log Messages

These rules are ALWAYS ACTIVE for internal API route handlers that validate webhook secrets, request processing functions that extract secrets from query parameters, and security validation logic that compares request secrets against environment configuration.

### Rules

- **R-CONSOLE-LOG-001** SHOULD: Error log messages SHOULD clearly identify the validation failure reason without exposing secret values.

### Verify

```bash
# Discover the project's test suite location and execute tests that validate internal API handlers reject requests with invalid or missing revalidation secrets
find . -type f -name '*.test.*' -o -name '*.spec.*' | grep -i 'api\|handler\|webhook' | head -5

# Locate and run the project's security validation test cases to confirm console error output appears for authentication failures
grep -r "console\.error" --include="*.test.*" --include="*.spec.*" . | grep -i 'secret\|auth\|validation' | head -10

# Identify the project's integration test configuration and verify webhook handler behavior with invalid secrets produces expected console error messages
grep -r "invalid.*secret\|missing.*secret" --include="*.test.*" --include="*.spec.*" . | head -10

# Verify internal API handlers reject requests with invalid revalidation secrets
grep -r "revalidation.*secret\|webhook.*secret" --include="*.js" --include="*.ts" . | grep -v node_modules | head -10

# Confirm console error statements do not expose actual secret values
grep -r "console\.error" --include="*.js" --include="*.ts" . | grep -v node_modules | grep -i 'secret\|auth' | grep -v 'secret values\|without.*secret' | head -10
```

**Accept when:**
- Internal API handlers reject requests with invalid revalidation secrets and produce console error output describing the validation failure
- Console error messages do not expose actual secret values, tokens, or sensitive configuration in their output
- Test coverage confirms that all secret validation failure paths produce appropriate console error logging

<enforcement>
Clause Code MUST NOT skip or defer verification. All three acceptance criteria must be confirmed before code is merged.
</enforcement>