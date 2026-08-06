# Access Environment Variables Through Process Object for Configuration: Default Values Provided

These rules are ALWAYS ACTIVE for server-side modules, API integration layers, utility modules, and component rendering logic that access external service credentials, authentication tokens, site metadata, and webhook validation secrets through the process object's environment property.

### Rules

- **R-ENV-001** MAY: Default values MAY be provided for non-sensitive configuration variables when environment values are absent.
- **R-ENV-002** MUST: Environment variable access MUST occur only in server-side execution contexts, never in client-side JavaScript bundles or static site generation contexts.
- **R-ENV-003** MUST: All modules that access environment variables MUST document the expected variable names, types, and whether they are required or optional.
- **R-ENV-004** MUST: Validation logic MUST execute during application initialization to verify required environment variables are present and contain valid values.
- **R-ENV-005** SHOULD: Implement startup validation that checks for required environment variables and fails fast with clear error messages.
- **R-ENV-006** SHOULD: Consider introducing schema validation or typed configuration wrappers for critical configuration values to maintain type safety.
- **R-EX-001** MAY: Public, non-sensitive configuration values may be exposed to client contexts when explicitly prefixed with a public indicator, subject to security team approval.

### Verify

```bash
# Discover the project's dependency manifest and identify the testing framework
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' \) | head -1

# Locate and execute the project's test suite to verify environment variable handling
# (Command varies by build tool; inspect manifest to determine)

# Discover the project's linting configuration and execute static analysis
# to detect any environment variable access in client-side contexts
grep -r "process\.env" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . 2>/dev/null | grep -E "(client|browser|public)" || echo "No client-side env access detected"

# Discover the project's build configuration and execute a production build
# to verify no secrets are embedded in client bundles
# (Command varies by build tool; inspect manifest to determine)
```

**Accept when:**
- All tests pass including those that verify environment variable validation and error handling for missing configuration
- Static analysis confirms no environment variable access occurs in client-side execution contexts
- Production build verification shows no sensitive environment variables embedded in client-accessible bundles
- All required environment variables are documented with their expected types and optionality
- Startup validation logic is present and tested for missing or invalid configuration scenarios

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that accesses environment variables through the process object. Violations MUST result in build failures, test failures, or code review rejection as specified in the enforcement section of the source ADR.
</enforcement>