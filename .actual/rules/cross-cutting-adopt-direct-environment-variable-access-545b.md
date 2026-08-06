# Adopt Direct Environment Variable Access for Runtime Configuration Secrets

These rules are ALWAYS ACTIVE for all server-side execution contexts, API integration code, component rendering logic, and any module accessing sensitive configuration values including API credentials, domain identifiers, and revalidation secrets.

### Rules

- **R-ENV-001** MUST: Retrieve secrets at the point of use within server-side execution contexts only.
- **R-ENV-002** MUST: Access environment variables directly through the process runtime object for credential-based authentication.
- **R-ENV-003** MUST NOT: Expose environment variable access in client-side code or component rendering logic.
- **R-ENV-004** SHOULD: Centralize secret retrieval patterns across boundary layer modules to maintain consistency.

### Verify

```bash
# Check for direct process.env access in server-side files
grep -r "process\.env" --include="*.ts" --include="*.js" src/ | grep -v "node_modules" | head -20

# Verify no environment variable access in client components
grep -r "process\.env" --include="*.tsx" --include="*.jsx" src/components/ 2>/dev/null || echo "No env access in components (good)"

# List all environment variable references to audit scope
grep -r "process\.env\.[A-Z_]*" --include="*.ts" --include="*.js" src/ | cut -d: -f2 | sort -u
```

**Accept when:**
- All environment variable access occurs in server-side execution contexts only
- No `process.env` references exist in client-side component files
- Secret retrieval happens at the point of use, not during module initialization for client code
- API credentials and domain identifiers are accessed only within boundary layer modules
- Credential-based authentication uses environment variables retrieved at request time

<enforcement>
Claude Code MUST NOT skip or defer verification. All server-side files must be audited for proper environment variable scoping before approval.
</enforcement>