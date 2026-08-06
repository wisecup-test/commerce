# Source Runtime Configuration from Process Environment Variables: Sensitive Credentials Including

These rules are ALWAYS ACTIVE for all server-side runtime code including API routes, server components, middleware, and configuration access for external service integration.

### Rules

- **R-CRED-001** MUST: Sensitive credentials including API tokens, access keys, and authentication secrets MUST be accessed exclusively through environment variables and MUST NOT be hardcoded in source files.

### Verify

```bash
# Discover the project's dependency manifest and identify the runtime environment
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -5

# Locate any validation utilities or startup scripts that verify environment variable presence
grep -r "process\.env\|os\.environ\|getenv" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . 2>/dev/null | grep -i "validat\|check\|require" | head -10

# Search the codebase for hardcoded credentials or sensitive values
grep -r "api[_-]?key\|secret\|token\|password" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . 2>/dev/null | grep -v "process\.env\|os\.environ\|getenv" | grep -v "node_modules" | grep -v ".git" | head -20

# Verify environment variable access patterns are consistent
grep -r "process\.env\[\|os\.environ\[\|getenv(" --include="*.js" --include="*.ts" --include="*.py" --include="*.go" . 2>/dev/null | wc -l

# Check for test configuration that validates missing environment variables
find . -path "*/test*" -o -path "*/spec*" | xargs grep -l "process\.env\|os\.environ" 2>/dev/null | head -5
```

**Accept when:**
- All environment variable access uses the standard runtime environment interface without hardcoded fallback values for sensitive credentials.
- Validation utilities or startup checks verify the presence of required environment variables and fail with clear error messages when configuration is incomplete.
- No sensitive credentials are hardcoded in source files or committed to version control.
- Configuration access patterns are consistent across all modules that require runtime configuration values.
- Integration tests verify application behavior with missing or malformed environment variables.

<enforcement>
Claude Code MUST NOT skip or defer verification of R-CRED-001. Hardcoded credentials or sensitive values discovered during review MUST be flagged for immediate removal and rotation.
</enforcement>