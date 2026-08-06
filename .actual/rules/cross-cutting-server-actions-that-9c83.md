# Adopt Async Function Exports as Standard Concurrency Model for Server Components and Actions: Server Actions That

These rules are ALWAYS ACTIVE for all server-side component rendering, data fetching, and server action implementations.

### Rules

- **R-ASYNC-001** SHOULD: Server actions that perform multiple asynchronous operations SHOULD structure await calls to reflect data dependencies and execution order requirements.

### Verify

```bash
# Discover the project's test runner configuration and execute the test suite that validates server component rendering and server action behavior
# Discover the project's static analysis tooling and run type checking to verify all async function exports have proper return type annotations
# Discover the project's linting configuration and execute linting rules that enforce await usage for Promise-returning operations
```

**Accept when:**
- All server component exports that perform I/O operations are declared as async functions and properly await asynchronous operations
- All server action exports that mutate state or call external services are declared as async functions with comprehensive error handling
- Type checking passes without errors related to Promise handling or async function return types

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated type checking in continuous integration pipeline verifies async function signatures and await usage. Code review process validates that all server-side I/O operations use async/await patterns. Integration tests verify proper execution order and error handling in async server actions.
</enforcement>