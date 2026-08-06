# Adopt Async Function Exports as Standard Concurrency Model for Server Components and Actions: Async Functions That

These rules are ALWAYS ACTIVE for all server-side component rendering, data fetching, and server action implementations.

### Rules

- **R-ASYNC-001** SHOULD: Async functions that access runtime configuration sources SHOULD await configuration retrieval before proceeding with business logic.

### Verify

```bash
# Discover the project's test runner configuration and execute the test suite that validates server component rendering and server action behavior
# (Exact command depends on project's build tool and test runner — derive from package.json or build manifest)

# Discover the project's static analysis tooling and run type checking to verify all async function exports have proper return type annotations
# (Exact command depends on project's type checker — derive from tsconfig.json or similar)

# Discover the project's linting configuration and execute linting rules that enforce await usage for Promise-returning operations
# (Exact command depends on project's linter — derive from .eslintrc or similar)
```

**Accept when:**
- All server component exports that perform I/O operations are declared as async functions and properly await asynchronous operations
- All server action exports that mutate state or call external services are declared as async functions with comprehensive error handling
- Type checking passes without errors related to Promise handling or async function return types

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures block pull request merging until async function signatures are corrected. Code review identifies missing await statements and requires revision before approval. Integration test failures trigger investigation of async operation sequencing and error handling.
</enforcement>