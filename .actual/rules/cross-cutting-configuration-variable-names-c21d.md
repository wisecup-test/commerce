# Use Process Environment Variables as Primary Runtime Configuration Source: Configuration Variable Names

These rules are ALWAYS ACTIVE for all server-side runtime contexts including API routes, server components, middleware, image generation functions, and any code that accesses runtime configuration for external service integration, authentication credentials, and environment-specific settings.

### Rules

- **R-CONFIG-001** MUST: Configuration variable names SHALL use uppercase with underscore separators to distinguish environment variables from code identifiers.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' \) | head -1

# Locate startup validation or configuration loading scripts
find . -type f -name '*config*' -o -name '*startup*' -o -name '*init*' | grep -E '\.(js|ts|py|go|rb)$'

# Search for direct process.env accessor usage
grep -r 'process\.env\.' --include='*.js' --include='*.ts' --include='*.jsx' --include='*.tsx' | grep -v node_modules | grep -v '.next'

# Verify all environment variable accesses follow uppercase-with-underscores convention
grep -r 'process\.env\.[a-z]' --include='*.js' --include='*.ts' --include='*.jsx' --include='*.tsx' | grep -v node_modules | grep -v '.next' || echo 'No lowercase env vars found (compliant)'

# Search for hardcoded credentials or environment-specific values
grep -r -E '(password|secret|token|api[_-]?key|endpoint|domain)\s*[=:]\s*["\']' --include='*.js' --include='*.ts' --include='*.jsx' --include='*.tsx' | grep -v node_modules | grep -v '.next' | grep -v test || echo 'No hardcoded credentials found (compliant)'
```

**Accept when:**
- All runtime configuration values are sourced from process environment variables with no hardcoded credentials or environment-specific values in source code
- Configuration variable names use consistent uppercase-with-underscores naming convention and include descriptive prefixes
- Startup validation exists that checks for required environment variables and provides clear error messages for missing or invalid configuration
- No direct process.env accesses use lowercase or mixed-case variable names
- Static analysis scanning confirms no hardcoded secrets or environment-specific configuration values in source files

<enforcement>
Claude Code MUST NOT skip or defer verification. All configuration variable names MUST follow the uppercase-with-underscores convention. Violations must be identified and corrected before code acceptance.
</enforcement>