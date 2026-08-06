# Use Environment Variables for Runtime Configuration in Logging Context: Configuration Access Patterns

These rules are ALWAYS ACTIVE for all runtime configuration access patterns in logging contexts, cache layer operations, external client boundaries, and request validation code.

### Rules

- **R-CONFIG-001** SHOULD: Configuration access patterns should be consistent across logging contexts, cache layer operations, and external client boundaries.
- **R-CONFIG-002** MUST: All environment-specific configuration values must be sourced from environment variables with no hardcoded credentials or endpoints in source code.
- **R-CONFIG-003** MUST: Configuration validation failures must produce console error output with sufficient context for diagnosis without exposing sensitive values.
- **R-CONFIG-004** MUST: All required environment variables must be documented and deployment configurations must provide them consistently across environments.
- **R-CONFIG-005** SHOULD: Create a centralized configuration module that encapsulates environment variable access and provides typed accessors with validation, ensuring consistent error handling and logging across all configuration consumers.
- **R-CONFIG-006** SHOULD: Document all required environment variables including their purpose, expected format, and whether they are required or optional, maintaining this documentation alongside deployment configuration.
- **R-CONFIG-007** MAY: Consider implementing configuration validation at application startup for critical values while maintaining request-time validation for webhook-specific secrets that may only be used conditionally.
- **R-CONFIG-008** MUST: Establish and enforce a hierarchical naming convention with service or subsystem prefixes for environment variables and document all environment variables in a central registry.

### Verify

```bash
# Discover the project's dependency manifest and identify all environment variable references
grep -r "process\.env\|ENV\|getenv" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . | grep -v node_modules | head -20

# Confirm hierarchical naming convention is followed
grep -r "process\.env\.[A-Z_]*" --include="*.js" --include="*.ts" . | grep -v node_modules | sed 's/.*process\.env\.\([A-Z_]*\).*/\1/' | sort -u

# Locate and execute project test suite for configuration loading
find . -name "*.test.js" -o -name "*.test.ts" -o -name "*.spec.js" -o -name "*.spec.ts" | head -10

# Scan for hardcoded credentials or sensitive values
grep -r "password\|secret\|token\|key" --include="*.js" --include="*.ts" . | grep -v node_modules | grep -v "process\.env" | grep -v "//" | head -20

# Identify deployment configuration files
find . -name ".env*" -o -name "*deployment*" -o -name "*config*" -type f | grep -v node_modules | head -10

# Verify no hardcoded endpoints or domains
grep -r "http://\|https://" --include="*.js" --include="*.ts" . | grep -v node_modules | grep -v "process\.env" | grep -v "//" | head -20
```

**Accept when:**
- All environment-specific configuration values are sourced from environment variables with no hardcoded credentials or endpoints in source code
- Configuration validation failures produce console error output with sufficient context for diagnosis without exposing sensitive values
- All required environment variables are documented and deployment configurations provide them consistently across environments
- Environment variable naming follows a hierarchical convention with service or subsystem prefixes
- A centralized configuration module exists that encapsulates environment variable access with typed accessors and validation
- Static analysis scanning confirms no hardcoded secrets or configuration values exist in source control
- Deployment validation checks confirm all required environment variables are present before deployment proceeds

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and must be verified before accepting pull requests or deployments.
</enforcement>