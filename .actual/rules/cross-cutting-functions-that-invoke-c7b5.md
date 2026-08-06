# Adopt Async Function Signatures for External Service Integration: Functions That Invoke

These rules are ALWAYS ACTIVE for all runtime execution contexts that interact with external services or perform I/O operations, including functions performing HTTP fetch operations, accessing runtime request context, performing file system operations, and constructing responses that depend on async I/O.

### Rules

- **R-ASYNC-001** MUST: Functions that invoke async operations MUST await the resolution of Promise values before consuming the result.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that detect missing await keywords on Promise-returning expressions
# (Exact tool and command derived from project repository)

# Locate the project's test suite and execute integration tests that verify async service integration functions properly await external calls and handle errors
# (Exact test command derived from project repository)

# Inspect the runtime framework's documentation to identify all async context APIs and verify that application code consistently awaits their resolution
```

**Accept when:**
- All functions performing external service calls, file system I/O, or runtime context access use async function signatures
- Static analysis reports zero violations for missing await keywords on Promise-returning function calls
- Integration tests successfully verify that async operations complete and return resolved values rather than pending Promises

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block merge until missing await keywords are added or Promise handling is corrected.
</enforcement>