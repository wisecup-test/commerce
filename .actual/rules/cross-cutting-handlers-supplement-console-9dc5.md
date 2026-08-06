# Use Console Error Logging for Invalid Revalidation Secrets in Internal API Handlers: Handlers Supplement Console

These rules are ALWAYS ACTIVE for internal API route handlers that validate webhook secrets, request processing functions that extract secrets from query parameters, and security validation logic that compares request secrets against environment configuration.

### Rules

- **R-CONSOLE-001** MAY: Handlers MAY supplement console logging with structured observability when available.
- **R-CONSOLE-002** MUST: Console error messages clearly indicate validation failure without including the expected or received secret values in the output.
- **R-CONSOLE-003** MUST: All console error statements be reviewed to ensure they describe the failure without exposing actual secret values, tokens, or sensitive parameters.
- **R-CONSOLE-004** SHOULD: Consider wrapping console error calls in a utility function that enforces consistent error message formatting and prevents accidental secret exposure.
- **R-CONSOLE-005** MUST: Internal API handlers reject requests with invalid or missing revalidation secrets and produce console error output describing the validation failure.

### Verify

```bash
# Discover the project's test suite location and execute tests that validate internal API handlers reject requests with invalid or missing revalidation secrets
find . -type f -name "*.test.*" -o -name "*.spec.*" | grep -i handler | head -5

# Locate and run the project's security validation test cases to confirm console error output appears for authentication failures
grep -r "console\.error" --include="*.test.*" --include="*.spec.*" | grep -i secret | head -10

# Identify the project's integration test configuration and verify webhook handler behavior with invalid secrets produces expected console error messages
grep -r "invalid.*secret\|revalidation" --include="*.test.*" --include="*.spec.*" | head -10

# Search for internal API handlers that validate webhook secrets
grep -r "query.*secret\|secret.*query" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -v node_modules | head -10

# Verify console error calls do not expose secret values
grep -r "console\.error" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -i secret | grep -v "// " | head -10
```

**Accept when:**
- Internal API handlers reject requests with invalid revalidation secrets and produce console error output describing the validation failure
- Console error messages do not expose actual secret values, tokens, or sensitive configuration in their output
- Test coverage confirms that all secret validation failure paths produce appropriate console error logging
- Code review verification confirms console error statements do not expose secret values
- Automated test suite execution confirms error logging behavior for invalid secrets

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for pull requests affecting internal API handlers that validate webhook secrets. Violations must be corrected before merge.
</enforcement>