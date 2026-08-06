# Use Process Environment Variables as Primary Runtime Configuration Source: Configuration Variables Use

These rules are ALWAYS ACTIVE for all server-side runtime contexts including API routes, server components, middleware, image generation functions, and any code that requires runtime configuration for external service integration, authentication credentials, or environment-specific settings.

### Rules

- **R-CONFIG-001** SHOULD: Configuration variables SHOULD use descriptive prefixes that identify the service or domain they configure.
- **R-CONFIG-002** MUST: All runtime configuration values MUST be sourced from process environment variables with no hardcoded credentials or environment-specific values in source code.
- **R-CONFIG-003** MUST: Configuration variable names MUST use consistent uppercase-with-underscores naming convention and include descriptive prefixes.
- **R-CONFIG-004** MUST: Startup validation MUST exist that checks for required environment variables and provides clear error messages for missing or invalid configuration.
- **R-CONFIG-005** MUST: Configuration accessed outside the approved pattern (centralized typed accessor functions) MUST be refactored before merge.
- **R-CONFIG-006** SHOULD: A centralized configuration module SHOULD be created that accesses all environment variables through typed accessor functions with validation.
- **R-CONFIG-007** SHOULD: All required and optional environment variables SHOULD be documented in a dedicated configuration reference with expected formats, example values, and validation rules.
- **R-CONFIG-008** MUST: Logging filters MUST redact sensitive configuration values to prevent exposure through error messages or logs.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' \) | head -1

# Locate startup validation or configuration loading scripts
find . -type f \( -name '*config*' -o -name '*startup*' -o -name '*init*' \) -path '*/src/*' | grep -E '\.(ts|js|py|go)$'

# Search for direct process.env accessor usage
grep -r 'process\.env\.' --include='*.ts' --include='*.js' | grep -v node_modules | grep -v '.next'

# Verify configuration variable naming convention (uppercase with underscores)
grep -r 'process\.env\.[A-Z_]*' --include='*.ts' --include='*.js' | grep -v node_modules | grep -v '.next'

# Search for hardcoded credentials or environment-specific values
grep -r -E '(password|secret|token|key|credential)\s*[:=]\s*["\']' --include='*.ts' --include='*.js' | grep -v node_modules | grep -v '.next' | grep -v test

# Locate configuration reference documentation
find . -type f \( -name '*CONFIG*' -o -name '*ENV*' -o -name '.env.example' \) | head -5

# Identify testing framework and locate configuration tests
grep -E '(jest|mocha|vitest|pytest)' package.json 2>/dev/null || echo 'Testing framework not found in package.json'
find . -type f -path '*/test*' -o -path '*/__tests__/*' | grep -E 'config|env' | head -5
```

**Accept when:**
- All runtime configuration values are sourced from process environment variables with no hardcoded credentials or environment-specific values in source code
- Configuration variable names use consistent uppercase-with-underscores naming convention and include descriptive prefixes identifying the service or domain
- A centralized configuration module exists that accesses environment variables through typed accessor functions with validation
- Startup validation exists that checks for required environment variables and provides clear error messages for missing or invalid configuration
- All required and optional environment variables are documented in a dedicated configuration reference
- No direct process.env accesses exist outside the centralized configuration module
- Logging and error handling redact sensitive configuration values
- Integration tests verify application behavior with different environment variable configurations

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and must be verified before accepting changes that introduce or modify runtime configuration access patterns.
</enforcement>