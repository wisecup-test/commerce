# Adopt Direct Environment Variable Access for Runtime Configuration Secrets

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-ENV-001** SHOULD: Modules that access multiple related secrets MUST validate the presence of required environment variables at initialization time.

### Verify

(no verify commands defined)

**Accept when:**
- Modules accessing multiple related secrets validate their presence at initialization time
- Environment variables are accessed directly through the process runtime object
- Sensitive configuration values (API credentials, domain identifiers, revalidation secrets) are retrieved at request time or initialization time as appropriate
- External service integration requirements are met through credential-based authentication

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>