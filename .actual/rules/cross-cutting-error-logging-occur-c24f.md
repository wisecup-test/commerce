# Adopt Console-Based Error Logging for Async Server Actions: Error Logging Occur

These rules are ALWAYS ACTIVE for all server-side async functions exported as public API contracts, including cart mutation operations, external API client functions, webhook handlers, and functions coordinating cache layer operations and cookie management.

### Rules

- **R-ASYNC-LOG-001** MUST: Error logging MUST occur within catch blocks of try-catch statements wrapping async operations.

### Verify

```bash
# Discover and execute the project's static analysis or linting verification to confirm all async server actions contain try-catch blocks with error logging
# Discover and run the project's test suite to verify error logging behavior in cart operations and external API client functions
# Discover and inspect the project's server action exports to confirm console.error usage in exception handling paths
```

**Accept when:**
- All async server actions performing cart mutations, external API calls, or webhook processing contain try-catch blocks with console.error logging
- Error log statements preserve the original exception object to maintain stack traces
- Static analysis or linting verification passes without violations of error logging requirements

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review rejection applies for async server actions missing error logging in exception handlers. Static analysis failures block merge until error handling is added.
</enforcement>