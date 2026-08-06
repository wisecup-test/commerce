# Use Environment Variables for Runtime Configuration in Logging Context: Configuration Validation Failures

These rules are ALWAYS ACTIVE for all code that accesses runtime configuration for logging, external API clients, request validation, cache layer operations, and boundary service code.

### Rules

- **R-CONFIG-001** MUST: Configuration validation failures must be logged using console error output to provide visibility into misconfiguration at request time.
- **R-CONFIG-002** MUST: All environment-specific configuration values must be sourced from environment variables with no hardcoded credentials or endpoints in source code.
- **R-CONFIG-003** MUST: Configuration error logging must not expose sensitive values; all sensitive configuration must be redacted in log output.
- **R-CONFIG-004** SHOULD: Create a centralized configuration module that encapsulates environment variable access and provides typed accessors with validation, ensuring consistent error handling and logging across all configuration consumers.
- **R-CONFIG-005** SHOULD: Document all required environment variables including their purpose, expected format, and whether they are required or optional, maintaining this documentation alongside deployment configuration.
- **R-CONFIG-006** SHOULD: Implement configuration validation at application startup for critical values while maintaining request-time validation for webhook-specific secrets that may only be used conditionally.
- **R-CONFIG-007** MAY: Development-only configuration may use hardcoded defaults if clearly marked and excluded from production builds.

### Verify

```bash
# Discover the project's dependency manifest and identify all environment variable references
grep -r "process\.env\|ENV\|getenv" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . | grep -v node_modules | grep -v \.next

# Scan for hardcoded credentials or configuration values
grep -r "http://\|https://" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . | grep -v node_modules | grep -v "http://localhost" | grep -v "https://example"

# Locate and execute the project's test suite for configuration loading
npm test -- --testPathPattern=config

# Identify deployment configuration and verify environment variables are documented
find . -name "*.env*" -o -name "docker-compose*" -o -name "*.yml" -o -name "*.yaml" | grep -v node_modules

# Verify no sensitive values are logged in configuration error paths
grep -r "console\.error\|logger\.error" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . | grep -i config
```

**Accept when:**
- All environment-specific configuration values are sourced from environment variables with no hardcoded credentials or endpoints in source code
- Configuration validation failures produce console error output with sufficient context for diagnosis without exposing sensitive values
- All required environment variables are documented and deployment configurations provide them consistently across environments
- No hardcoded credentials or environment-specific endpoints exist in source control
- Automated static analysis scanning confirms no hardcoded secrets or configuration values are present
- Deployment validation checks confirm all required environment variables are present before deployment proceeds

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before accepting code changes. Static analysis and deployment validation are non-negotiable enforcement mechanisms.
</enforcement>