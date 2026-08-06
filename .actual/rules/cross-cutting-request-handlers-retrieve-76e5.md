# Use Console Error Logging for Invalid Revalidation Secrets in Internal API Handlers: Request Handlers Retrieve

These rules are ALWAYS ACTIVE for internal API route handlers that validate webhook secrets, request processing functions that extract secrets from query parameters, and security validation logic that compares request secrets against environment configuration.

### Rules

- **R-CONSOLE-001** MUST: Request handlers MUST retrieve cache identifiers from cookies and topic identifiers from headers before validation.
- **R-CONSOLE-002** MUST: Console error messages MUST NOT expose actual secret values, tokens, or sensitive configuration in their output.
- **R-CONSOLE-003** MUST: All secret validation failure paths MUST produce appropriate console error logging describing the validation failure.
- **R-CONSOLE-004** SHOULD: Wrap console error calls in a utility function that enforces consistent error message formatting and prevents accidental secret exposure.

### Verify

```bash
# Discover the project's test suite location and execute tests that validate internal API handlers reject requests with invalid or missing revalidation secrets
find . -type f -name "*.test.*" -o -name "*.spec.*" | grep -i "handler\|webhook\|secret" | head -5

# Locate and run the project's security validation test cases to confirm console error output appears for authentication failures
grep -r "console\.error" --include="*.test.*" --include="*.spec.*" | grep -i "secret\|validation\|auth" | head -10

# Identify the project's integration test configuration and verify webhook handler behavior with invalid secrets produces expected console error messages
grep -r "invalid.*secret\|missing.*secret" --include="*.test.*" --include="*.spec.*" | head -10

# Verify no secret values are exposed in console error statements
grep -r "console\.error" --include="*.js" --include="*.ts" | grep -v "node_modules" | xargs grep -l "secret\|token" | xargs grep -E "console\.error.*\$\{.*secret\}|console\.error.*secret.*value" || echo "No exposed secrets found in console.error calls"
```

**Accept when:**
- Internal API handlers reject requests with invalid revalidation secrets and produce console error output describing the validation failure
- Console error messages do not expose actual secret values, tokens, or sensitive configuration in their output
- Test coverage confirms that all secret validation failure paths produce appropriate console error logging
- Request handlers retrieve cache identifiers from cookies and topic identifiers from headers before performing validation

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that falls within the defined scope.
</enforcement>