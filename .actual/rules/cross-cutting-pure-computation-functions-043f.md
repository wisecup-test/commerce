# Adopt Async Function Signatures for External Service Integration: Pure Computation Functions

These rules are ALWAYS ACTIVE for all runtime execution contexts that interact with external services or perform I/O operations.

### Rules

- **R-ASYNC-001** MUST: All functions performing HTTP fetch operations to external service endpoints use async function signatures.
- **R-ASYNC-002** MUST: All functions accessing runtime request context including cookies and headers use async function signatures.
- **R-ASYNC-003** MUST: All functions performing file system read operations use async function signatures.
- **R-ASYNC-004** MUST: All public API contracts exported for service integration use async function signatures.
- **R-ASYNC-005** MUST: All functions constructing responses that depend on async I/O operations use async function signatures.
- **R-ASYNC-006** MAY: Pure computation functions that do not perform I/O MAY use synchronous signatures even when called from async contexts.
- **R-ASYNC-007** MUST: All await keywords on Promise-returning expressions be present and explicit in function bodies.
- **R-ASYNC-008** MUST: All async function call sites properly await the function and handle potential Promise rejections.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that detect missing await keywords on Promise-returning expressions
npm run lint -- --rule "no-floating-promises"

# Locate the project's test suite and execute integration tests that verify async service integration functions properly await external calls and handle errors
npm test -- --testPathPattern="integration|service"

# Inspect the runtime framework's documentation to identify all async context APIs and verify that application code consistently awaits their resolution
grep -r "cookies()\|headers()" src/ --include="*.ts" --include="*.tsx" | grep -v "await"
```

**Accept when:**
- All functions performing external service calls, file system I/O, or runtime context access use async function signatures
- Static analysis reports zero violations for missing await keywords on Promise-returning function calls
- Integration tests successfully verify that async operations complete and return resolved values rather than pending Promises
- Code review confirms that new service integration functions use async signatures
- No unhandled Promise rejections are detected in runtime monitoring

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block merge until missing await keywords are added or Promise handling is corrected. Code review must identify synchronous blocking I/O in request paths and require refactoring to async patterns.
</enforcement>