# Access Environment Variables Through Process Object for Runtime Configuration: Non Sensitive Configuration

These rules are ALWAYS ACTIVE for all modules that construct HTTP requests to external services, all components that render dynamic content based on deployment environment, all utility functions that validate or transform configuration values, and all API route handlers that verify request authenticity using secrets.

### Rules

- **R-ENV-001** MUST: Access all runtime configuration values including credentials through the process environment property and never hardcode secrets in source files.
- **R-ENV-002** MUST: Implement validation logic that checks for required environment variables at application startup and fails fast with clear error messages when they are missing.
- **R-ENV-003** SHOULD: Create typed configuration objects that parse and validate environment variable values at startup, providing type-safe access throughout the application.
- **R-ENV-004** SHOULD: Document all required environment variables in a template or example file that developers can copy to set up their local development environment.
- **R-ENV-005** MAY: Access non-sensitive configuration values such as site names and feature flags through the same process environment mechanism for consistency.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' | head -1

# Locate any validation or startup scripts that check environment variable presence
grep -r 'process\.env' --include='*.ts' --include='*.js' --include='*.tsx' --include='*.jsx' | grep -E '(validation|startup|config)' | head -20

# Search the codebase for all references to the process environment property
grep -r 'process\.env' --include='*.ts' --include='*.js' --include='*.tsx' --include='*.jsx' | wc -l

# Verify that no credentials are hardcoded in source files
grep -r -E '(password|secret|token|key)\s*[=:]\s*["\']' --include='*.ts' --include='*.js' --include='*.tsx' --include='*.jsx' | grep -v node_modules | grep -v '.actual'

# Identify the project's testing framework and locate test suites
grep -r 'describe\|test\|it(' --include='*.test.ts' --include='*.test.js' --include='*.spec.ts' --include='*.spec.js' | head -5

# Verify configuration loading and validation tests exist
grep -r 'process\.env' --include='*.test.ts' --include='*.test.js' --include='*.spec.ts' --include='*.spec.js' | head -10
```

**Accept when:**
- All configuration values including credentials are accessed through the process environment property and no hardcoded secrets exist in source files
- Validation logic exists that checks for required environment variables and provides clear error messages when they are missing
- Tests verify that the application handles missing or invalid environment variables appropriately
- A centralized configuration module or validation function exists that exports typed configuration objects
- Required environment variables are documented in a template or example file

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that falls within the defined scope.
</enforcement>