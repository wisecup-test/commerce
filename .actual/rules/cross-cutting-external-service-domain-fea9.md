# Use Environment Variables for Runtime Configuration in Logging Context: External Service Domain

These rules are ALWAYS ACTIVE for all code that accesses external service domain endpoints, access tokens, webhook validation secrets, and runtime configuration consumed by logging, cache layer operations, and external client boundary code.

### Rules

- **R-ENV-001** MUST: External service domain endpoints, access tokens, and validation secrets must be read from distinct environment variables with descriptive names indicating their purpose.

### Verify

```bash
# Discover the project's dependency manifest and identify all environment variable references
grep -r "process\.env\|getenv\|ENV\[" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . | grep -v node_modules | head -20

# Confirm no hardcoded credentials or environment-specific endpoints exist in source code
grep -r "https://" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" . | grep -v node_modules | grep -v "http://localhost" | grep -v test | head -20

# Locate and execute project test suite for configuration loading
find . -name "*.test.*" -o -name "*.spec.*" | head -10

# Identify deployment configuration and verify environment variables are documented
find . -name ".env*" -o -name "*deploy*" -o -name "*config*" | grep -v node_modules | head -20
```

**Accept when:**
- All environment-specific configuration values are sourced from environment variables with no hardcoded credentials or endpoints in source code
- Configuration validation failures produce console error output with sufficient context for diagnosis without exposing sensitive values
- All required environment variables are documented and deployment configurations provide them consistently across environments
- Environment variable names follow a hierarchical naming convention with descriptive purpose indicators
- No hardcoded API endpoints, tokens, or secrets appear in application code

<enforcement>
Claude Code MUST NOT skip or defer verification. All environment variable access MUST be validated against this rule before code is committed.
</enforcement>