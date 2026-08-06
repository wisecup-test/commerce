# Access Runtime Configuration Through Process Environment Variables: Modules Consuming Configuration

These rules are ALWAYS ACTIVE for library modules, server-side rendering components, utility functions, and API route handlers that consume runtime configuration values.

### Rules

- **R-CONFIG-001** MUST: Modules consuming configuration values MUST access them directly from the process environment at point of use.
- **R-CONFIG-002** MUST: Access environment variables through the process environment object using property access syntax with the configuration key name.
- **R-CONFIG-003** MUST: Implement validation functions that check for required configuration values and provide clear error messages when values are missing.
- **R-CONFIG-004** SHOULD: Consider providing default values for optional configuration while requiring explicit values for service credentials and external integrations.
- **R-CONFIG-005** SHOULD: Follow uppercase underscore-separated naming convention for configuration key names.

### Verify

```bash
# Discover the project's dependency manifest and identify modules that access process environment variables
find . -type f \( -name "*.js" -o -name "*.ts" -o -name "*.jsx" -o -name "*.tsx" \) -exec grep -l "process\.env" {} \;

# Locate and execute the project's verification scripts that validate environment configuration schema
if [ -f "scripts/verify-config" ] || [ -f "scripts/validate-env" ]; then
  ls -la scripts/ | grep -E "(verify|validate|config)"
fi

# Inspect the codebase for direct process environment access patterns and verify consistent key naming
grep -r "process\.env\.[A-Z_]*" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | cut -d: -f2 | sort | uniq -c
```

**Accept when:**
- All modules accessing runtime configuration use the process environment object consistently
- Configuration key names follow uppercase underscore-separated naming convention
- Required configuration values are validated at appropriate lifecycle points
- Validation functions provide clear error messages for missing required configuration
- No hardcoded configuration values exist in library code, components, or utility modules

<enforcement>
Clause MUST NOT skip or defer verification. Code review MUST verify configuration access patterns. Static analysis MUST detect direct process environment access. Runtime validation tests MUST confirm configuration availability.
</enforcement>