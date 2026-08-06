# Access Runtime Configuration Through Process Environment Variables: Runtime Configuration Values

These rules are ALWAYS ACTIVE for library modules integrating with external services, server-side rendering components requiring site metadata, utility functions performing environment-specific operations, and API route handlers requiring service credentials.

### Rules

- **R-CONFIG-001** MUST: Runtime configuration values MUST be accessed through the process environment object using property access syntax with the configuration key name.
- **R-CONFIG-002** MUST: Configuration key names MUST follow uppercase underscore-separated naming convention.
- **R-CONFIG-003** MUST: Required configuration values MUST be validated at appropriate lifecycle points (application startup or module initialization).
- **R-CONFIG-004** SHOULD: Implement validation functions that check for required configuration values and provide clear error messages when values are missing.
- **R-CONFIG-005** SHOULD: Consider providing default values for optional configuration while requiring explicit values for service credentials and external integrations.
- **R-CONFIG-006** SHOULD: Implement error handling that masks sensitive values and provides safe diagnostic information when configuration access fails.

### Verify

```bash
# Discover the project's dependency manifest and identify modules that access process environment variables
find . -type f \( -name "*.js" -o -name "*.ts" -o -name "*.jsx" -o -name "*.tsx" \) -exec grep -l "process\.env" {} \;

# Locate and execute the project's verification scripts that validate environment configuration schema
if [ -f "package.json" ]; then npm run verify:config 2>/dev/null || echo "No verify:config script found"; fi

# Inspect the codebase for direct process environment access patterns and verify consistent key naming
grep -r "process\.env\.[A-Z_]*" --include="*.js" --include="*.ts" --include="*.jsx" --include="*.tsx" | grep -v node_modules | sort | uniq
```

**Accept when:**
- All modules accessing runtime configuration use the process environment object consistently
- Configuration key names follow uppercase underscore-separated naming convention
- Required configuration values are validated at appropriate lifecycle points
- Error handling masks sensitive values in logs and error messages
- Configuration access patterns are documented and reviewed

<enforcement>
Claude Code MUST NOT skip or defer verification. All configuration access patterns must be validated against these rules before code is committed.
</enforcement>