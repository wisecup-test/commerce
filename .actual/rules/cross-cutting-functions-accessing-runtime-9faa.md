# Adopt Async Function Signatures for External Service Integration: Functions Accessing Runtime

These rules are ALWAYS ACTIVE for all runtime execution contexts that interact with external services or perform I/O operations.

### Rules

- **R-ASYNC-001** SHOULD: Functions accessing runtime configuration from environment sources SHOULD use async patterns when the runtime framework provides async context APIs.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that detect missing await keywords on Promise-returning expressions
# Locate the project's test suite and execute integration tests that verify async service integration functions properly await external calls and handle errors
# Inspect the runtime framework's documentation to identify all async context APIs and verify that application code consistently awaits their resolution
```

**Accept when:**
- All functions performing external service calls, file system I/O, or runtime context access use async function signatures
- Static analysis reports zero violations for missing await keywords on Promise-returning function calls
- Integration tests successfully verify that async operations complete and return resolved values rather than pending Promises

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block merge until missing await keywords are added or Promise handling is corrected. Code review identifies synchronous blocking I/O in request paths and requires refactoring to async patterns.
</enforcement>