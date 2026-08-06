# Use Process Environment Variables as Primary Runtime Configuration Source: Security Sensitive Values

These rules are ALWAYS ACTIVE for all server-side runtime contexts including API routes, server components, middleware, and image generation functions that require runtime configuration and security-sensitive credentials.

### Rules

- **R-ENV-001** MUST: Security-sensitive values including access tokens, API keys, and secrets SHALL be sourced exclusively from process environment variables and never hardcoded in source code.
- **R-ENV-002** MUST: All runtime configuration values accessed in server-side contexts SHALL use the process environment variable accessor pattern with no hardcoded credentials or environment-specific values.
- **R-ENV-003** SHOULD: Configuration variable names use consistent uppercase-with-underscores naming convention and include descriptive prefixes that identify the service or purpose.
- **R-ENV-004** SHOULD: Create a centralized configuration module that accesses all environment variables through typed accessor functions with validation, providing a single source of truth for configuration schema.
- **R-ENV-005** SHOULD: Implement startup validation that checks for required environment variables before the application begins serving requests, failing fast with actionable error messages.
- **R-ENV-006** SHOULD: Document all required and optional environment variables in a dedicated configuration reference, including expected formats, example values, and validation rules.
- **R-ENV-007** MAY: Exceptions for non-sensitive default values may be approved by the technical lead with documentation of the rationale in the configuration reference.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' \) | head -1

# Locate startup validation or configuration loading scripts
find . -type f \( -name '*config*' -o -name '*startup*' -o -name '*init*' \) -path '*/src/*' | grep -E '\.(ts|js|py|go)$'

# Search for direct process.env accessor usage
grep -r 'process\.env\.' --include='*.ts' --include='*.js' src/ | grep -v node_modules | head -20

# Verify configuration variable naming convention (uppercase with underscores)
grep -r 'process\.env\.[A-Z_]*' --include='*.ts' --include='*.js' src/ | grep -v node_modules

# Search for hardcoded credentials or environment-specific values
grep -r -E '(password|secret|token|key|credential)\s*[:=]\s*["\']' --include='*.ts' --include='*.js' src/ | grep -v node_modules | grep -v test | grep -v '.env.example'

# Locate test files that verify configuration loading behavior
find . -type f -name '*.test.ts' -o -name '*.test.js' -o -name '*.spec.ts' -o -name '*.spec.js' | xargs grep -l 'process\.env\|config' 2>/dev/null | head -10
```

**Accept when:**
- All runtime configuration values are sourced from process environment variables with no hardcoded credentials or environment-specific values in source code
- Configuration variable names use consistent uppercase-with-underscores naming convention and include descriptive prefixes
- Startup validation exists that checks for required environment variables and provides clear error messages for missing or invalid configuration
- A centralized configuration module or accessor pattern is implemented for typed environment variable access
- All required and optional environment variables are documented with expected formats and example values
- No hardcoded secrets, API keys, tokens, or credentials appear in source files outside of test fixtures or example configurations

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for server-side runtime contexts. Static analysis and code review verification are required before accepting pull requests that modify configuration access patterns or introduce new environment-dependent behavior.
</enforcement>