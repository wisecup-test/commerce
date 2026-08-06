# Access Environment Variables Through Process Environment Interface: Modules That Consume

These rules are ALWAYS ACTIVE for all modules that consume environment variables, including API client modules, UI components, utility functions, webhook handlers, and configuration validation logic.

### Rules

- **R-ENV-001** SHOULD: Modules that consume environment variables SHOULD validate the presence and format of required configuration values at initialization.

### Verify

```bash
# Discover the project's environment variable validation script in the repository and execute it to verify all required configuration values are defined
find . -type f -name '*env*' -o -name '*config*' | grep -E '\.(js|ts|py|sh)$' | head -5

# Locate the project's test suite and run integration tests that verify environment variable access patterns across all modules
find . -type f -name '*.test.*' -o -name '*.spec.*' | head -5

# Identify the project's static analysis or linting configuration and execute checks that detect hardcoded credentials or configuration values in source files
grep -r 'process\.env\|ENV\|environment' --include='*.js' --include='*.ts' --include='*.py' . 2>/dev/null | head -10
```

**Accept when:**
- All required environment variables are successfully retrieved through the process environment interface without runtime errors
- No hardcoded credentials, tokens, or sensitive configuration values are present in source files
- Environment variable validation logic correctly identifies missing or invalid configuration at startup

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools MUST scan source files for hardcoded credentials. Code review MUST verify new configuration values use environment variables. Integration tests MUST validate environment variable access patterns. Build pipeline MUST fail if hardcoded credentials are detected. Runtime validation MUST fail fast at startup if required environment variables are missing.
</enforcement>