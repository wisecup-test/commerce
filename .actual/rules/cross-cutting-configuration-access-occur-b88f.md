# Use Process Environment Variables as Primary Runtime Configuration Source: Configuration Access Occur

These rules are ALWAYS ACTIVE for all server-side runtime contexts including API routes, server components, middleware, image generation functions, and dynamic content rendering that require external service integration, authentication credentials, or environment-specific configuration.

### Rules

- **R-CONFIG-001** SHOULD: Configuration access SHOULD occur at the point of use rather than being cached at module initialization to support dynamic reconfiguration.
- **R-CONFIG-002** MUST: All runtime configuration values MUST be sourced from process environment variables with no hardcoded credentials or environment-specific values in source code.
- **R-CONFIG-003** MUST: Configuration variable names MUST use consistent uppercase-with-underscores naming convention and include descriptive prefixes.
- **R-CONFIG-004** MUST: Startup validation MUST exist that checks for required environment variables and provides clear error messages for missing or invalid configuration.
- **R-CONFIG-005** MUST: Create a centralized configuration module that accesses all environment variables through typed accessor functions with validation.
- **R-CONFIG-006** MUST: Document all required and optional environment variables in a dedicated configuration reference, including expected formats, example values, and validation rules.
- **R-CONFIG-007** MUST: Implement logging filters to redact sensitive values and avoid including configuration in error messages.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' \) | head -1

# Locate any startup validation or configuration loading scripts
find . -type f -name '*config*' -o -name '*startup*' -o -name '*init*' | grep -E '\.(js|ts|py|go)$' | head -10

# Search for direct process.env accessor usage
grep -r 'process\.env\.' --include='*.js' --include='*.ts' --include='*.jsx' --include='*.tsx' | grep -v node_modules | head -20

# Verify configuration variable naming convention (uppercase with underscores)
grep -r 'process\.env\.[A-Z_]+' --include='*.js' --include='*.ts' --include='*.jsx' --include='*.tsx' | grep -v node_modules

# Search for hardcoded credentials or environment-specific values
grep -r -E '(password|secret|token|api[_-]?key|endpoint)\s*[=:]\s*["\']' --include='*.js' --include='*.ts' --include='*.jsx' --include='*.tsx' | grep -v node_modules | grep -v test | grep -v '.actual'

# Locate configuration reference documentation
find . -type f -name '*CONFIG*' -o -name '*config*' | grep -E '\.(md|txt|yml|yaml)$' | head -5
```

**Accept when:**
- All runtime configuration values are sourced from process environment variables with no hardcoded credentials or environment-specific values in source code
- Configuration variable names use consistent uppercase-with-underscores naming convention and include descriptive prefixes
- Startup validation exists that checks for required environment variables and provides clear error messages for missing or invalid configuration
- A centralized configuration module with typed accessor functions exists
- Configuration reference documentation is maintained with all required and optional variables
- Logging and error handling redact sensitive configuration values

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for server-side runtime configuration access patterns.
</enforcement>