# Adopt Direct Environment Variable Access for Runtime Configuration Secrets

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Use uppercase naming with underscore separators for environment variable names containing secrets to distinguish configuration from code variables.
- **R-ENV-002** MUST: Access sensitive configuration values (API credentials, domain identifiers, revalidation secrets) directly from environment variables at request time or initialization time.
- **R-ENV-003** MUST: Ensure environment variable access for secrets is consistent across server-side API integration code and component rendering logic.

### Verify

```bash
# Check for environment variable access patterns in the codebase
grep -r "process\.env\.[A-Z_]*" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" .

# Verify uppercase naming convention for secret environment variables
grep -r "process\.env\.[a-z]" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . | grep -i "secret\|key\|token\|credential\|api" || echo "No lowercase secret env vars found"

# Confirm no hardcoded credentials in source files
grep -r "(password|secret|token|key)\s*=\s*['\"]" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . || echo "No hardcoded credentials detected"
```

**Accept when:**
- All environment variable names for secrets use uppercase with underscore separators
- Sensitive configuration is accessed via `process.env` at runtime, not hardcoded
- Access patterns are consistent across server-side and component rendering contexts
- No plaintext credentials appear in source code

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>