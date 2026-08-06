# Access Environment Variables Through Process Object for Configuration: Missing Required Environment

These rules are ALWAYS ACTIVE for server-side modules, API integration layers, utility modules, and component rendering logic that access external service credentials, authentication tokens, site metadata, and webhook signature validation through environment variables.

### Rules

- **R-ENV-001** SHOULD: Missing required environment variables SHOULD be detected and reported during application initialization.
- **R-ENV-002** MUST: Environment variable access MUST only occur in server-side execution contexts, never in client-side JavaScript bundles.
- **R-ENV-003** SHOULD: All modules that access environment variables SHOULD be documented with expected variable names, types, and whether they are required or optional.
- **R-ENV-004** MUST: Validation logic MUST execute during application initialization to verify required environment variables are present and contain valid values.
- **R-ENV-005** MAY: Public, non-sensitive configuration values MAY be exposed to client contexts when explicitly prefixed with a public indicator (EX-001).

### Verify

```bash
# Discover the project's dependency manifest and identify the testing framework
find . -maxdepth 2 -type f \( -name "package.json" -o -name "pyproject.toml" -o -name "go.mod" -o -name "Gemfile" \) | head -1

# Locate and execute the project's test suite to verify environment variable handling
# (command varies by build tool; consult project's build configuration)

# Discover the project's linting configuration and execute static analysis
# to detect any environment variable access in client-side contexts
grep -r "process\.env" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . 2>/dev/null | grep -E "(client|browser|public)" || echo "No client-side env access detected"

# Discover the project's build configuration and execute a production build
# to verify no secrets are embedded in client bundles
# (command varies by build tool; consult project's build configuration)
```

**Accept when:**
- All tests pass including those that verify environment variable validation and error handling for missing configuration
- Static analysis confirms no environment variable access occurs in client-side execution contexts
- Production build verification shows no sensitive environment variables embedded in client-accessible bundles
- All required environment variables are documented with their expected names, types, and requirement status
- Application initialization includes validation that fails fast with clear error messages for missing required variables

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code touching environment variable access patterns. Violations must be caught during code review and static analysis phases before merge.
</enforcement>