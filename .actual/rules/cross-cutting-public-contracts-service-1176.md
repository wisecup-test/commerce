# Adopt Async Function Signatures for External Service Integration: Public Contracts Service

These rules are ALWAYS ACTIVE for all runtime execution contexts that interact with external services or perform I/O operations.

### Rules

- **R-ASYNC-001** MUST: Public API contracts for service integration MUST expose async function signatures to enable caller coordination of concurrent operations.
- **R-ASYNC-002** MUST: All functions performing HTTP fetch operations to external service endpoints MUST use async function signatures.
- **R-ASYNC-003** MUST: All functions accessing runtime request context including cookies and headers MUST use async function signatures.
- **R-ASYNC-004** MUST: All functions performing file system read operations MUST use async function signatures.
- **R-ASYNC-005** MUST: All functions constructing responses that depend on async I/O operations MUST use async function signatures.
- **R-ASYNC-006** MUST: When implementing new service integration functions, declare the function signature as async and ensure all I/O operations within the function body use await for Promise resolution.
- **R-ASYNC-007** MUST: For functions that access runtime request context, await the context accessor functions before extracting values, and propagate the async signature to all callers in the chain.
- **R-ASYNC-008** SHOULD: When refactoring existing synchronous code to async patterns, audit all call sites to ensure they properly await the function and handle potential Promise rejections.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that detect missing await keywords on Promise-returning expressions
npm run lint -- --rule "no-floating-promises"

# Locate the project's test suite and execute integration tests that verify async service integration functions properly await external calls and handle errors
npm run test:integration

# Inspect the runtime framework's documentation to identify all async context APIs and verify that application code consistently awaits their resolution
grep -r "cookies()\|headers()" src/ | grep -v "await"
```

**Accept when:**
- All functions performing external service calls, file system I/O, or runtime context access use async function signatures
- Static analysis reports zero violations for missing await keywords on Promise-returning function calls
- Integration tests successfully verify that async operations complete and return resolved values rather than pending Promises

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block merge until missing await keywords are added or Promise handling is corrected.
</enforcement>