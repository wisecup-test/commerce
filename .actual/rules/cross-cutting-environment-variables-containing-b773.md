# Access Environment Variables Through Process Object for Configuration: Environment Variables Containing

These rules are ALWAYS ACTIVE for all server-side modules, API integration layers, utility modules, and component rendering logic that access runtime configuration through environment variables.

### Rules

- **R-ENV-001** MUST_NOT: Environment variables containing secrets SHALL NOT be accessed in client-side execution contexts.
- **R-ENV-002** MUST: Identify all modules that access environment variables and document the expected variable names, types, and whether they are required or optional.
- **R-ENV-003** MUST: Implement validation logic that executes during application initialization to verify required environment variables are present and contain valid values.
- **R-ENV-004** MUST: Ensure environment variable access only occurs in server-side execution contexts by reviewing module boundaries and execution environment.
- **R-ENV-005** MAY: Public, non-sensitive configuration values may be exposed to client contexts when explicitly prefixed with a public indicator (EX-001).

### Verify

```bash
# Discover the project's dependency manifest and identify the testing framework
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' | head -1

# Locate and execute the project's test suite to verify environment variable handling
# (command varies by build tool; consult project's build configuration)

# Discover the project's linting configuration and execute static analysis
# to detect any environment variable access in client-side contexts
grep -r "process\.env" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -E "(client|browser|public)" || echo "No client-side env access detected"

# Discover the project's build configuration and execute a production build
# to verify no secrets are embedded in client bundles
# (command varies by build tool; consult project's build configuration)
```

**Accept when:**
- All tests pass including those that verify environment variable validation and error handling for missing configuration
- Static analysis confirms no environment variable access occurs in client-side execution contexts
- Production build verification shows no sensitive environment variables embedded in client-accessible bundles
- Required environment variables are documented with their expected types and validation requirements
- Application initialization includes startup validation that fails fast with clear error messages for missing configuration

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code touching environment variable access patterns. Static analysis and build verification MUST complete successfully before accepting changes.
</enforcement>