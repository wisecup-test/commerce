# Use Process Environment Variables as Primary Runtime Configuration Source: Default Values Provided

These rules are ALWAYS ACTIVE for all server-side runtime contexts including API routes, server components, middleware, image generation functions, and dynamic content rendering that require runtime configuration and external service integration.

### Rules

- **R-ENV-001** MUST: Source all runtime configuration values from process environment variables with no hardcoded credentials or environment-specific values in source code.
- **R-ENV-002** MUST: Use consistent uppercase-with-underscores naming convention for all environment variable names with descriptive prefixes.
- **R-ENV-003** MUST: Implement startup validation that checks for required environment variables before the application begins serving requests, failing fast with actionable error messages that identify missing or invalid configuration.
- **R-ENV-004** SHOULD: Create a centralized configuration module that accesses all environment variables through typed accessor functions with validation, providing a single source of truth for configuration schema.
- **R-ENV-005** SHOULD: Document all required and optional environment variables in a dedicated configuration reference, including expected formats, example values, and validation rules.
- **R-ENV-006** MAY: Provide default values for non-critical configuration using object spread patterns when environment variables are undefined.
- **R-ENV-007** MUST: Implement logging filters to redact sensitive values and avoid including configuration in error messages to prevent secrets exposure through error messages, logging, or process inspection.
- **R-ENV-008** MUST: Create typed configuration accessor functions that parse and validate environment variable values with appropriate error handling and type conversion to mitigate type coercion errors.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' \) | head -1

# Locate any startup validation or configuration loading scripts
find . -type f \( -name '*config*' -o -name '*startup*' -o -name '*init*' \) -path '*/src/*' | grep -E '\.(ts|js|py|go)$'

# Search for direct process.env accessor usage
grep -r 'process\.env\.' --include='*.ts' --include='*.js' | grep -v node_modules | grep -v '.next'

# Verify all accesses follow naming convention (UPPERCASE_WITH_UNDERSCORES)
grep -r 'process\.env\.[a-z]' --include='*.ts' --include='*.js' | grep -v node_modules | grep -v '.next'

# Search for hardcoded credentials or environment-specific values
grep -r -E '(password|secret|token|key|credential)\s*[=:]\s*["\']' --include='*.ts' --include='*.js' | grep -v node_modules | grep -v '.next' | grep -v test

# Identify the project's testing framework
grep -E '(jest|mocha|vitest|pytest|rspec)' package.json 2>/dev/null || grep -E '(jest|mocha|vitest|pytest|rspec)' requirements.txt 2>/dev/null

# Locate test files that verify configuration loading behavior
find . -type f \( -name '*.test.ts' -o -name '*.test.js' -o -name '*.spec.ts' -o -name '*.spec.js' \) | xargs grep -l 'process\.env\|config' 2>/dev/null
```

**Accept when:**
- All runtime configuration values are sourced from process environment variables with no hardcoded credentials or environment-specific values in source code
- Configuration variable names use consistent uppercase-with-underscores naming convention and include descriptive prefixes
- Startup validation exists that checks for required environment variables and provides clear error messages for missing or invalid configuration
- A centralized configuration module with typed accessor functions exists and is used consistently across the codebase
- Configuration reference documentation exists listing all required and optional environment variables with formats and examples
- Logging and error handling filters are in place to prevent secrets exposure
- Integration tests verify application behavior with different environment variable configurations

<enforcement>
Clause MUST NOT skip or defer verification. All rules marked MUST are mandatory before code merge. Rules marked SHOULD are strongly recommended. Rules marked MAY are optional but preferred. Static analysis and code review must confirm compliance with R-ENV-001, R-ENV-002, R-ENV-003, R-ENV-007, and R-ENV-008 before acceptance.
</enforcement>