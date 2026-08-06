# Adopt Console-Based Error Logging for Async Server Actions: Async Server Actions

These rules are ALWAYS ACTIVE for all async server actions that perform cart mutations, external API calls, webhook processing, or coordinate cache layer operations and cookie management.

### Rules

- **R-ASYNC-001** MUST: All async server actions that perform cart mutations, external API calls, or webhook processing MUST log caught exceptions to the console error stream.
- **R-ASYNC-002** MUST: Wrap all async operations that interact with external services, cache layers, or cookies in try-catch blocks with console.error logging in the catch clause.
- **R-ASYNC-003** MUST: Preserve the original exception object when logging to maintain stack traces and error metadata for debugging.
- **R-ASYNC-004** SHOULD: Consider adding contextual information to error logs such as operation type, cart ID, or merchandise ID to aid in debugging distributed async workflows.

### Verify

```bash
# Discover and execute the project's static analysis or linting verification to confirm all async server actions contain try-catch blocks with error logging
# (Command discovery required from project repository)

# Discover and run the project's test suite to verify error logging behavior in cart operations and external API client functions
# (Command discovery required from project repository)

# Discover and inspect the project's server action exports to confirm console.error usage in exception handling paths
# (Command discovery required from project repository)
```

**Accept when:**
- All async server actions performing cart mutations, external API calls, or webhook processing contain try-catch blocks with console.error logging
- Error log statements preserve the original exception object to maintain stack traces
- Static analysis or linting verification passes without violations of error logging requirements
- Contextual information (operation type, cart ID, merchandise ID) is included in error logs where applicable

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review rejection is required for async server actions missing error logging in exception handlers. Static analysis failures block merge until error handling is added.
</enforcement>