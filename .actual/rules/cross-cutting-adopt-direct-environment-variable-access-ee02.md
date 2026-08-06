# Adopt Direct Environment Variable Access for Runtime Configuration Secrets

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** MUST: Access sensitive configuration values (API credentials, domain identifiers, revalidation secrets) directly through environment variables at runtime.
- **R-ENV-002** SHOULD: Isolate secret access to server-side rendering functions rather than client components.
- **R-ENV-003** MUST: Retrieve secrets at request time and initialization time as required by external service integration.

### Verify

```bash
# Check that environment variable access is isolated to server-side code
grep -r "process\.env" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" | grep -v "server" | grep -v "api" | wc -l

# Verify no direct environment variable access in client components
grep -r "process\.env" src/components/client --include="*.tsx" --include="*.ts" 2>/dev/null | wc -l

# Confirm server-side rendering functions access secrets appropriately
grep -r "process\.env" src/server --include="*.ts" --include="*.tsx" | head -20
```

**Accept when:**
- Environment variables are accessed only in server-side code paths
- Client components do not directly access `process.env`
- Secrets are retrieved at request time or initialization time as needed
- External service integrations receive credentials through environment variables

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>