# Use Environment Variables for Runtime Configuration in Logging Context: Environment Variable Names

These rules are ALWAYS ACTIVE for all runtime configuration code that accesses external service APIs, logging context, cache layer operations, request validation, and boundary service code consuming domain endpoints, access tokens, and webhook validation secrets.

### Rules

- **R-ENV-001** SHOULD: Environment variable names should follow a hierarchical naming convention that groups related configuration by service or subsystem.
- **R-ENV-002** MUST: All environment-specific configuration values must be sourced from environment variables with no hardcoded credentials or endpoints in source code.
- **R-ENV-003** MUST: Configuration validation failures must produce console error output with sufficient context for diagnosis without exposing sensitive values.
- **R-ENV-004** MUST: All required environment variables must be documented and deployment configurations must provide them consistently across environments.
- **R-ENV-005** SHOULD: Create a centralized configuration module that encapsulates environment variable access and provides typed accessors with validation, ensuring consistent error handling and logging across all configuration consumers.
- **R-ENV-006** SHOULD: Document all required environment variables including their purpose, expected format, and whether they are required or optional, maintaining this documentation alongside deployment configuration.
- **R-ENV-007** MAY: Consider implementing configuration validation at application startup for critical values while maintaining request-time validation for webhook-specific secrets that may only be used conditionally.

### Verify

```bash
# Discover the project's dependency manifest and identify all environment variable references
grep -r "process\.env\|ENV\|getenv" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . | grep -v node_modules | head -20

# Confirm hierarchical naming convention is followed
grep -r "process\.env\.[A-Z_]*" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . | grep -v node_modules | sort | uniq

# Verify no hardcoded credentials or endpoints exist
grep -r "http://\|https://" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . | grep -v node_modules | grep -v "//" | head -10

# Locate and execute configuration-related tests
find . -name "*.test.*" -o -name "*.spec.*" | xargs grep -l "config\|environment" | head -5

# Identify deployment configuration files
find . -name ".env*" -o -name "*deployment*" -o -name "*docker-compose*" -o -name "*k8s*" | head -10
```

**Accept when:**
- All environment-specific configuration values are sourced from environment variables with no hardcoded credentials or endpoints in source code
- Configuration validation failures produce console error output with sufficient context for diagnosis without exposing sensitive values
- All required environment variables are documented and deployment configurations provide them consistently across environments
- Environment variable names follow a hierarchical naming convention grouping related configuration by service or subsystem
- A centralized configuration module exists that encapsulates environment variable access with typed accessors and validation
- All configuration access includes error logging with sufficient context

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and deployment validation. Static analysis scanning for hardcoded secrets must be performed before merge. Deployment failures due to missing environment variables block promotion to production until configuration is corrected.
</enforcement>