# Adopt Console-Based Error Logging for Async Server Actions: Error Logging Include

These rules are ALWAYS ACTIVE for all server-side async functions exported as public API contracts, including cart mutation operations, external API client functions, webhook handlers, and functions that coordinate cache layer operations and cookie management.

### Rules

- **R-ASYNC-LOG-001** MUST: Wrap all async operations that interact with external services, cache layers, or cookies in try-catch blocks with console.error logging in the catch clause.
- **R-ASYNC-LOG-002** MUST: Preserve the original exception object when logging to maintain stack traces and error metadata for debugging.
- **R-ASYNC-LOG-003** MAY: Error logging MAY include contextual information about the operation being performed when the error occurred (such as operation type, cart ID, or merchandise ID).
- **R-ASYNC-LOG-004** MUST: Implement error sanitization before logging to ensure exception objects do not contain values from security.secrets_handling sources.

### Verify

```bash
# Discover and execute the project's static analysis or linting verification to confirm all async server actions contain try-catch blocks with error logging
# (Command to be discovered from project configuration)

# Discover and run the project's test suite to verify error logging behavior in cart operations and external API client functions
# (Command to be discovered from project configuration)

# Discover and inspect the project's server action exports to confirm console.error usage in exception handling paths
# (Command to be discovered from project configuration)
```

**Accept when:**
- All async server actions performing cart mutations, external API calls, or webhook processing contain try-catch blocks with console.error logging.
- Error log statements preserve the original exception object to maintain stack traces.
- Static analysis or linting verification passes without violations of error logging requirements.
- Error sanitization is implemented to prevent exposure of sensitive data in logged exceptions.

<enforcement>
Claude Code MUST NOT skip or defer verification. All async server actions in scope MUST contain try-catch blocks with console.error logging before code is accepted.
</enforcement>