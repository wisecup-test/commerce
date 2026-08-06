# Access Environment Variables Through Process Object for Configuration: Environment Variable Names

These rules are ALWAYS ACTIVE for all server-side modules, API integration layers, utility modules, and component rendering logic that access external service credentials, authentication tokens, site metadata, or webhook validation secrets through the process object.

### Rules

- **R-ENV-001** MUST: Environment variable names for secrets SHALL use uppercase naming with underscore separators.
- **R-ENV-002** MUST: Environment variable access SHALL only occur in server-side execution contexts, never in client-side JavaScript bundles.
- **R-ENV-003** MUST: Required environment variables SHALL be validated during application initialization with clear error messages for missing configuration.
- **R-ENV-004** SHOULD: Document all modules that access environment variables, including expected variable names, types, and whether they are required or optional.
- **R-ENV-005** MAY: Public, non-sensitive configuration values may be exposed to client contexts when explicitly prefixed with a public indicator (EX-001).

### Verify

```bash
# 1. Discover the project's dependency manifest and identify the testing framework
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' \) | head -1

# 2. Locate and execute the project's test suite to verify environment variable handling
# (Command varies by build tool; consult project's build configuration)

# 3. Discover the project's linting configuration and execute static analysis
# to detect any environment variable access in client-side contexts
# (Command varies by linter; consult project's linting setup)

# 4. Discover the project's build configuration and execute a production build
# to verify no secrets are embedded in client bundles
# (Command varies by build tool; consult project's build configuration)

# 5. Search for environment variable access patterns
grep -r 'process\.env' --include='*.js' --include='*.ts' --include='*.jsx' --include='*.tsx' . | grep -v node_modules

# 6. Verify uppercase naming convention for secret environment variables
grep -r 'process\.env\.[a-z]' --include='*.js' --include='*.ts' --include='*.jsx' --include='*.tsx' . | grep -v node_modules
```

**Accept when:**
- All tests pass including those that verify environment variable validation and error handling for missing configuration
- Static analysis confirms no environment variable access occurs in client-side execution contexts
- Production build verification shows no sensitive environment variables embedded in client-accessible bundles
- All environment variable names for secrets follow uppercase naming with underscore separators
- Required environment variables are validated at application startup with clear error messages
- All modules accessing environment variables are documented with expected variable names, types, and requirement status

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules R-ENV-001 through R-ENV-005 must be verified before accepting changes that introduce or modify environment variable access patterns.
</enforcement>