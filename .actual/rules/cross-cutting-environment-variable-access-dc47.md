# Access Environment Variables Through Process Object for Configuration: Environment Variable Access

These rules are ALWAYS ACTIVE for all server-side modules, API integration layers, utility modules, and component rendering logic that access external service credentials, authentication tokens, site metadata, or webhook validation secrets.

### Rules

- **R-ENV-001** SHOULD: Environment variable access SHOULD be centralized in configuration or utility modules rather than scattered throughout business logic.
- **R-ENV-002** MUST: Environment variable access MUST only occur in server-side execution contexts; client-side JavaScript bundles MUST NOT reference process.env.
- **R-ENV-003** MUST: Required environment variables MUST be validated during application initialization with clear error messages for missing or invalid values.
- **R-ENV-004** SHOULD: Sensitive configuration values SHOULD remain external to source code to reduce risk of accidental exposure in version control.
- **R-ENV-005** MAY: Public, non-sensitive configuration values MAY be exposed to client contexts when explicitly prefixed with a public indicator (EX-001).

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

# Validate that required environment variables are checked at startup
grep -r "process\.env" --include="*.js" --include="*.ts" . 2>/dev/null | grep -E "(validation|required|check|assert)" || echo "Review startup validation logic"
```

**Accept when:**
- All tests pass including those that verify environment variable validation and error handling for missing configuration
- Static analysis confirms no environment variable access occurs in client-side execution contexts
- Production build verification shows no sensitive environment variables embedded in client-accessible bundles
- Required environment variables are validated at application startup with clear error messages
- Environment variable access is centralized in configuration or utility modules with documented expected variable names, types, and requirements

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that accesses environment variables in server-side contexts. Static analysis for client-side environment variable leakage MUST be performed before accepting changes. Startup validation logic MUST be reviewed and confirmed present.
</enforcement>