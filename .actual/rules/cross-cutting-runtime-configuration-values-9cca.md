# Access Environment Variables Through Process Object for Configuration: Runtime Configuration Values

These rules are ALWAYS ACTIVE for all server-side modules, API integration layers, utility modules, and component rendering logic that require access to runtime configuration values, credentials, and secrets.

### Rules

- **R-CONFIG-001** MUST: All runtime configuration values including credentials SHALL be accessed through the process object's environment property.
- **R-CONFIG-002** MUST: Environment variable access SHALL only occur in server-side execution contexts, never in client-side JavaScript bundles.
- **R-CONFIG-003** MUST: All modules that access environment variables SHALL be documented with expected variable names, types, and whether they are required or optional.
- **R-CONFIG-004** MUST: Validation logic that verifies required environment variables are present and contain valid values SHALL execute during application initialization.
- **R-CONFIG-005** SHOULD: Implement startup validation that checks for required environment variables and fails fast with clear error messages.
- **R-CONFIG-006** MAY: Public, non-sensitive configuration values may be exposed to client contexts when explicitly prefixed with a public indicator (EX-001).

### Verify

```bash
# Discover the project's dependency manifest and identify the testing framework
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' \) | head -1

# Locate and execute the project's test suite to verify environment variable handling
# (Command varies by build tool; inspect manifest to determine)

# Discover the project's linting configuration and execute static analysis
# to detect any environment variable access in client-side contexts
grep -r "process\.env" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . 2>/dev/null | grep -E "(client|browser|public)" || echo "No client-side env access detected"

# Discover the project's build configuration and execute a production build
# to verify no secrets are embedded in client bundles
# (Command varies by build tool; inspect manifest to determine)

# Verify environment variable validation exists
grep -r "process\.env" --include="*.js" --include="*.ts" . 2>/dev/null | grep -E "(validate|check|required|throw|Error)" | head -5
```

**Accept when:**
- All tests pass including those that verify environment variable validation and error handling for missing configuration
- Static analysis confirms no environment variable access occurs in client-side execution contexts
- Production build verification shows no sensitive environment variables embedded in client-accessible bundles
- All required environment variables are documented with their expected names, types, and optionality
- Startup validation logic is present and executes before application initialization completes

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that accesses runtime configuration values. Violations must be caught during code review and static analysis phases.
</enforcement>