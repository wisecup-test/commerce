# Access Runtime Configuration Through Process Environment Variables: Sensitive Configuration Values

These rules are ALWAYS ACTIVE for library modules integrating with external services, server-side rendering components requiring site metadata, utility functions performing environment-specific operations, and API route handlers requiring service credentials.

### Rules

- **R-CONFIG-001** SHOULD: Sensitive configuration values SHOULD be distinguished from public metadata through naming conventions or validation.
- **R-CONFIG-002** MUST: Access environment variables through the process environment object using property access syntax with the configuration key name.
- **R-CONFIG-003** MUST: Implement validation functions that check for required configuration values and provide clear error messages when values are missing.
- **R-CONFIG-004** SHOULD: Consider providing default values for optional configuration while requiring explicit values for service credentials and external integrations.
- **R-CONFIG-005** MUST: Configuration key names MUST follow uppercase underscore-separated naming convention.
- **R-CONFIG-006** MUST: Required configuration values MUST be validated at appropriate lifecycle points (application startup or module initialization).
- **R-CONFIG-007** MUST: Error handling MUST mask sensitive values and provide safe diagnostic information when configuration access fails.

### Verify

```bash
# Discover the project's dependency manifest and identify modules that access process environment variables
find . -type f \( -name "*.js" -o -name "*.ts" -o -name "*.jsx" -o -name "*.tsx" \) -exec grep -l "process\.env" {} \;

# Locate and execute the project's verification scripts that validate environment configuration schema
ls -la | grep -E "(test|verify|validate|check)" | grep -E "\.(js|ts|sh)$"

# Inspect the codebase for direct process environment access patterns and verify consistent key naming
grep -r "process\.env\.[A-Z_]*" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | sort | uniq

# Verify configuration validation is implemented
grep -r "validation\|validator\|validate" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -i config
```

**Accept when:**
- All modules accessing runtime configuration use the process environment object consistently
- Configuration key names follow uppercase underscore-separated naming convention
- Required configuration values are validated at appropriate lifecycle points
- Validation functions provide clear error messages for missing required values
- Sensitive values are masked in error messages and logs
- No hardcoded configuration values exist in source code

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and must be checked before accepting changes to configuration access patterns.
</enforcement>