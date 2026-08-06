# Access Runtime Configuration Through Process Environment Variables: Configuration Keys Use

These rules are ALWAYS ACTIVE for library modules integrating with external services, server-side rendering components requiring site metadata, utility functions performing environment-specific operations, and API route handlers requiring service credentials.

### Rules

- **R-CONFIG-001** MUST: Configuration keys MUST use uppercase naming with underscore separators.
- **R-CONFIG-002** MUST: Access environment variables through the process environment object using property access syntax with the configuration key name.
- **R-CONFIG-003** MUST: Implement validation functions that check for required configuration values and provide clear error messages when values are missing.
- **R-CONFIG-004** SHOULD: Consider providing default values for optional configuration while requiring explicit values for service credentials and external integrations.

### Verify

```bash
# Discover the project's dependency manifest and identify modules that access process environment variables
find . -type f \( -name 'package.json' -o -name 'pyproject.toml' -o -name 'go.mod' -o -name 'Gemfile' \) | head -1

# Locate and execute the project's verification scripts that validate environment configuration schema
find . -type f -name '*test*' -o -name '*verify*' | grep -i config | head -5

# Inspect the codebase for direct process environment access patterns and verify consistent key naming
grep -r 'process\.env\|ENV\[\|os\.environ' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' . | grep -E '[A-Z_]+' | head -20
```

**Accept when:**
- All modules accessing runtime configuration use the process environment object consistently
- Configuration key names follow uppercase underscore-separated naming convention
- Required configuration values are validated at appropriate lifecycle points
- Validation functions provide clear error messages for missing configuration

<enforcement>
Clause Code MUST NOT skip or defer verification. Configuration key naming and environment access patterns MUST be validated before accepting changes to configuration access code.
</enforcement>