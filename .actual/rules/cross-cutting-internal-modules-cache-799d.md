# Source Runtime Configuration from Environment Variables for External Service Integration: Internal Modules Cache

These rules are ALWAYS ACTIVE for all internal API modules that integrate with external services, webhook handlers that validate incoming requests, and configuration initialization code that prepares API client instances.

### Rules

- **R-CACHE-001** MAY: Internal API modules MAY cache configuration values at module initialization to avoid repeated environment lookups during request processing.
- **R-CACHE-002** MUST: Implement startup validation that reads all required configuration keys and fails immediately with descriptive errors if any are missing or empty, preventing partial initialization.
- **R-CACHE-003** MUST: Source all credentials, endpoints, and secrets from environment variables without hardcoding values in source code.
- **R-CACHE-004** SHOULD: Group related configuration keys using a consistent prefix pattern that identifies the external service, making it clear which keys belong together.
- **R-CACHE-005** SHOULD: Implement a configuration module that encapsulates environment variable access and provides typed accessors, centralizing validation and error handling.
- **R-CACHE-006** MUST: Ensure webhook validation and API authentication both use the same configuration mechanism, demonstrating consistent pattern application.
- **R-CACHE-007** MUST: Implement logging and error handling policies that redact configuration values and only expose key names in diagnostic output.

### Verify

```bash
# Discover the project's dependency manifest and identify configuration validation tests
find . -name 'package.json' -o -name 'requirements.txt' -o -name 'go.mod' -o -name 'Gemfile' | head -1

# Locate integration test suites that exercise API functions with missing configuration
find . -path '*/test*' -o -path '*/spec*' | xargs grep -l 'environment\|config' 2>/dev/null | head -5

# Identify static analysis or linting rules that detect direct credential usage
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.golangci.yml' 2>/dev/null

# Search for hardcoded credentials or endpoints in API modules
grep -r 'https://' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' . 2>/dev/null | grep -v 'process.env\|os.environ\|getenv' | head -10

# Verify environment variable validation at startup
grep -r 'process.env\|os.environ\|getenv' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' . 2>/dev/null | grep -i 'required\|throw\|error\|panic' | head -10
```

**Accept when:**
- All internal API modules that integrate with external services source credentials and endpoints from environment variables without hardcoded values
- Startup validation confirms required configuration keys are present and fails fast with clear error messages identifying missing keys
- Webhook validation and API authentication both use the same configuration mechanism, demonstrating consistent pattern application
- Configuration values are redacted in logs and error messages, exposing only key names in diagnostic output
- Related configuration keys use a consistent prefix pattern identifying the external service
- A centralized configuration module encapsulates environment variable access with typed accessors

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review and must be checked before approving changes to internal API modules that integrate with external services.
</enforcement>