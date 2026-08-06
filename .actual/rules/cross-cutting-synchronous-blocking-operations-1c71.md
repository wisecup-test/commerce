# Adopt Async Function Signatures for External Service Integration: Synchronous Blocking Operations

These rules are ALWAYS ACTIVE for all runtime execution contexts that interact with external services or perform I/O operations, including all functions performing HTTP fetch operations to external service endpoints, all functions accessing runtime request context including cookies and headers, all functions performing file system read operations, all public API contracts exported for service integration, and all functions constructing responses that depend on async I/O operations.

### Rules

- **R-ASYNC-001** MUST NOT: Synchronous blocking operations MUST NOT be used for network I/O, external service calls, or runtime context access in request-handling paths.
- **R-ASYNC-002** MUST: All functions performing external service calls, file system I/O, or runtime context access use async function signatures.
- **R-ASYNC-003** MUST: All I/O operations within async function bodies use await for Promise resolution.
- **R-ASYNC-004** MUST: All callers of async functions properly await the function and handle potential Promise rejections.
- **R-ASYNC-005** SHOULD: Pure utility functions performing synchronous data transformations remain synchronous.
- **R-ASYNC-006** SHOULD: Type guard functions operating on in-memory values remain synchronous.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that detect missing await keywords on Promise-returning expressions
npm run lint -- --rule "no-floating-promises"

# Locate the project's test suite and execute integration tests that verify async service integration functions properly await external calls and handle errors
npm test -- --testPathPattern="integration|service"

# Inspect the runtime framework's documentation to identify all async context APIs and verify that application code consistently awaits their resolution
grep -r "cookies()\|headers()" src/ | grep -v "await" || echo "All context accessors properly awaited"
```

**Accept when:**
- All functions performing external service calls, file system I/O, or runtime context access use async function signatures
- Static analysis reports zero violations for missing await keywords on Promise-returning function calls
- Integration tests successfully verify that async operations complete and return resolved values rather than pending Promises
- All runtime context accessors (cookies(), headers()) are consistently awaited throughout the codebase

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block merge until missing await keywords are added or Promise handling is corrected. Code review must identify synchronous blocking I/O in request paths and require refactoring to async patterns.
</enforcement>