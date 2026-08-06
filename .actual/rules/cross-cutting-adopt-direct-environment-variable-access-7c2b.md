# Adopt Direct Environment Variable Access for Runtime Configuration Secrets

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Access all runtime configuration secrets through the process environment object rather than hardcoding them in source files.
- **R-ENV-002** MUST: Retrieve API credentials, domain identifiers, and revalidation secrets from environment variables at request time or initialization time.
- **R-ENV-003** SHOULD: Centralize environment variable access patterns across server-side API integration code and component rendering logic to maintain consistency.

### Verify

```bash
# Check for hardcoded secrets in source files
grep -r "password\|secret\|token\|key" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . | grep -v "process.env" | grep -v "node_modules" | grep -v ".actual"

# Verify environment variable access patterns
grep -r "process.env" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . | grep -v "node_modules" | wc -l

# Check for direct credential references in configuration files
grep -r "api_key\|apiKey\|API_KEY" --include="*.json" --include="*.config.js" . | grep -v "process.env" | grep -v "node_modules"
```

**Accept when:**
- No hardcoded secrets, API credentials, or domain identifiers are found in source files
- All sensitive configuration values are accessed through `process.env` at runtime
- Environment variable patterns are consistently applied across server-side and component code
- External service integrations retrieve credentials from the process environment object

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>