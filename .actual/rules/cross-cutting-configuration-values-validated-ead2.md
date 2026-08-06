# Use Environment Variables for Runtime Configuration in Logging Context: Configuration Values Validated

These rules are ALWAYS ACTIVE for all runtime configuration access in logging context, external API client boundary code, webhook validation handlers, and cache layer operations that consume environment-sourced configuration values.

### Rules

- **R-CONFIG-001** MAY: Configuration values may be validated at request time when used in webhook or callback handlers that execute conditionally.
- **R-CONFIG-002** MUST: All environment-specific configuration values must be sourced from environment variables with no hardcoded credentials or endpoints in source code.
- **R-CONFIG-003** MUST: Configuration validation failures must produce console error output with sufficient context for diagnosis without exposing sensitive values.
- **R-CONFIG-004** MUST: All required environment variables must be documented with their purpose, expected format, and whether they are required or optional.
- **R-CONFIG-005** SHOULD: Create a centralized configuration module that encapsulates environment variable access and provides typed accessors with validation, ensuring consistent error handling and logging across all configuration consumers.
- **R-CONFIG-006** SHOULD: Implement configuration validation at application startup for critical values while maintaining request-time validation for webhook-specific secrets that may only be used conditionally.
- **R-CONFIG-007** SHOULD: Establish and enforce a hierarchical naming convention with service or subsystem prefixes for environment variable names and maintain a central registry.

### Verify

```bash
# Discover the project's dependency manifest and identify all environment variable references
find . -type f \( -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' \) | head -1
grep -r 'process\.env\|os\.environ\|getenv' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' . 2>/dev/null | grep -v node_modules | head -20

# Locate and execute the project's test suite for configuration loading
find . -type f \( -name 'test' -o -name 'tests' -o -name 'spec' \) -o -name '*test*' -o -name '*spec*' | head -5

# Identify deployment configuration and verify environment variables are documented
find . -type f \( -name '.env*' -o -name 'docker-compose*' -o -name 'k8s*' -o -name 'terraform*' \) 2>/dev/null | head -10

# Scan for hardcoded credentials or sensitive values
grep -r 'password\|secret\|token\|key' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | grep -v node_modules | grep -v '.git' | head -20
```

**Accept when:**
- All environment-specific configuration values are sourced from environment variables with no hardcoded credentials or endpoints in source code
- Configuration validation failures produce console error output with sufficient context for diagnosis without exposing sensitive values
- All required environment variables are documented and deployment configurations provide them consistently across environments
- A centralized configuration module exists that encapsulates environment variable access with typed accessors and validation
- Environment variable naming follows a hierarchical convention with service or subsystem prefixes
- Critical configuration values are validated at application startup; webhook-specific secrets are validated at request time
- No hardcoded credentials, API endpoints, or environment-specific configuration exist in source control

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and deployment validation. Static analysis scanning for hardcoded secrets must be executed before merge. Deployment must be blocked if required environment variables are missing or if configuration validation fails.
</enforcement>