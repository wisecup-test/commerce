# Adopt Direct Environment Variable Access for Runtime Configuration Secrets

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Retrieve sensitive configuration values (API credentials, domain identifiers, revalidation secrets) directly from environment variables at request time or initialization time.
- **R-ENV-002** MUST NOT: Log secret values retrieved from environment variables.
- **R-ENV-003** MUST NOT: Serialize secret values to client responses.
- **R-ENV-004** MUST NOT: Expose secret values through public API contracts.
- **R-ENV-005** SHOULD: Access environment variables through the process runtime object consistently across server-side API integration code and component rendering logic.

### Verify

```bash
# Check for environment variable access patterns
grep -r "process\.env" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -E "(API|SECRET|KEY|CREDENTIAL|DOMAIN|REVALIDATION)" || echo "No direct env var access found"

# Check for secret logging patterns
grep -r "console\.log\|logger\." --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -E "(process\.env|secret|credential|token|key)" && echo "WARNING: Potential secret logging detected" || echo "No obvious secret logging found"

# Check for client-side secret exposure
grep -r "JSON\.stringify\|response\.json" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -E "(process\.env|secret|credential|token|key)" && echo "WARNING: Potential client exposure detected" || echo "No obvious client exposure found"
```

**Accept when:**
- Environment variables are accessed directly via `process.env` for sensitive configuration retrieval
- No secret values appear in console logs, error messages, or debug output
- No secret values are serialized into client-facing responses or API contracts
- Sensitive configuration is consistently retrieved at request time or initialization time
- All external service integrations use environment-sourced credentials

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>