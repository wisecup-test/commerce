# Use Environment Variables for Runtime Configuration in Logging Context: Environment Specific Configuration

These rules are ALWAYS ACTIVE for all code that accesses external service APIs, logging context, cache layer operations, request validation, and boundary service code that consumes runtime configuration values.

### Rules

- **R-ENV-001** MUST: All environment-specific configuration values required by logging, external clients, and request validation logic must be sourced from environment variables at runtime.
- **R-ENV-002** MUST: External API client configuration including domain endpoints and access tokens must be sourced from environment variables.
- **R-ENV-003** MUST: Webhook validation secrets used in request handlers must be sourced from environment variables.
- **R-ENV-004** MUST: Configuration values logged during error conditions must be sourced from environment variables.
- **R-ENV-005** MUST: Runtime configuration accessed by cache layer and boundary service code must be sourced from environment variables.
- **R-ENV-006** MUST: No hardcoded credentials or environment-specific endpoints may exist in source code.
- **R-ENV-007** SHOULD: Create a centralized configuration module that encapsulates environment variable access and provides typed accessors with validation.
- **R-ENV-008** SHOULD: Implement configuration validation at application startup for critical values while maintaining request-time validation for webhook-specific secrets.
- **R-ENV-009** SHOULD: Establish and enforce a hierarchical naming convention with service or subsystem prefixes for environment variables.
- **R-ENV-010** SHOULD: Document all required environment variables including their purpose, expected format, and whether they are required or optional.
- **R-ENV-011** SHOULD: Ensure all configuration error logging includes sufficient context for diagnosis without exposing sensitive values.

### Verify

```bash
# Discover the project's dependency manifest and identify all environment variable references
find . -type f \( -name '*.js' -o -name '*.ts' -o -name '*.jsx' -o -name '*.tsx' \) | xargs grep -l 'process\.env\|import\.meta\.env' | head -20

# Scan for hardcoded credentials or environment-specific endpoints
grep -r 'https://.*\.example\.com\|api_key.*=\|secret.*=\|token.*=' --include='*.js' --include='*.ts' --include='*.jsx' --include='*.tsx' . 2>/dev/null | grep -v node_modules | grep -v '.actual' || echo 'No obvious hardcoded credentials found'

# Locate and execute configuration-related tests
find . -type f -name '*.test.*' -o -name '*.spec.*' | xargs grep -l 'config\|environment' | head -5

# Identify deployment configuration files
find . -type f \( -name '.env*' -o -name 'docker-compose*' -o -name 'k8s*' -o -name 'terraform*' \) | head -10

# Check for environment variable documentation
find . -type f \( -name 'README*' -o -name 'DEPLOYMENT*' -o -name '.env.example' \) | xargs grep -l 'environment' 2>/dev/null || echo 'No environment documentation found'
```

**Accept when:**
- All environment-specific configuration values are sourced from environment variables with no hardcoded credentials or endpoints in source code
- Configuration validation failures produce console error output with sufficient context for diagnosis without exposing sensitive values
- All required environment variables are documented and deployment configurations provide them consistently across environments
- A centralized configuration module exists that encapsulates environment variable access with typed accessors and validation
- Environment variable naming follows a hierarchical convention with service or subsystem prefixes
- Static analysis confirms no hardcoded secrets or configuration values exist in the codebase
- Deployment validation checks confirm all required environment variables are present before deployment proceeds

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and must be verified before accepting pull requests or deployments.
</enforcement>