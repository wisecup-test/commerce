# Access Runtime Configuration Through Process Environment Variables: Utility Functions Provide

These rules are ALWAYS ACTIVE for library modules, server-side rendering components, utility functions, and API route handlers that require access to environment-specific configuration values.

### Rules

- **R-ENV-001** MAY: Utility functions MAY provide validation or transformation of environment configuration values.
- **R-ENV-002** MUST: Access environment variables through the process environment object using property access syntax with the configuration key name.
- **R-ENV-003** MUST: Implement validation functions that check for required configuration values and provide clear error messages when values are missing.
- **R-ENV-004** SHOULD: Consider providing default values for optional configuration while requiring explicit values for service credentials and external integrations.
- **R-ENV-005** MUST: Use uppercase underscore-separated naming convention for configuration key names.
- **R-ENV-006** MUST: Validate required configuration values at appropriate lifecycle points (application startup or module initialization).

### Verify

```bash
# Discover the project's dependency manifest and identify modules that access process environment variables
find . -type f \( -name "*.js" -o -name "*.ts" -o -name "*.jsx" -o -name "*.tsx" \) -exec grep -l "process\.env" {} \;

# Locate and execute the project's verification scripts that validate environment configuration schema
ls -la | grep -E "(test|verify|check|validate)"

# Inspect the codebase for direct process environment access patterns and verify consistent key naming
grep -r "process\.env\.[A-Z_]*" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | sort | uniq

# Verify configuration key names follow uppercase underscore-separated convention
grep -r "process\.env\.[a-z]" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" && echo "FAIL: Found lowercase env keys" || echo "PASS: All env keys use uppercase convention"
```

**Accept when:**
- All modules accessing runtime configuration use the process environment object consistently
- Configuration key names follow uppercase underscore-separated naming convention
- Required configuration values are validated at appropriate lifecycle points
- Validation functions provide clear error messages for missing required values
- Service credentials and external integration keys are required (not optional)
- Optional configuration values have documented defaults

<enforcement>
Claude Code MUST NOT skip or defer verification. All environment variable access patterns MUST be reviewed for consistency with this rule set before code is committed.
</enforcement>