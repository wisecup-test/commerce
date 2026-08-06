# Export Async Server Functions as Named Public Contracts: Async Function Contracts

These rules are ALWAYS ACTIVE for server action modules, page components, metadata generation functions, layout components, and API route handlers that perform server-side operations and coordinate async I/O, state mutations, and rendering.

### Rules

- **R-ASYNC-001** SHOULD: Async function contracts SHOULD separate concerns by delegating to internal helper functions for data access, external service calls, and transformation logic.
- **R-ASYNC-002** SHOULD: Establish naming conventions for async exports: server actions use imperative verbs describing state mutations, page components use descriptive nouns matching route structure, metadata generators follow framework-mandated function names.
- **R-ASYNC-003** SHOULD: Implement consistent error handling within async exports: wrap external service calls in try-catch blocks, log errors with context, return user-facing error messages or fallback values, and coordinate cache invalidation on failure.
- **R-ASYNC-004** SHOULD: Separate public async function exports from internal implementation by delegating to helper functions for data access, transformation, and external service coordination, enabling independent testing and evolution.

### Verify

```bash
# Discover the project's module system configuration and identify all files exporting async functions
# Verify that public contracts use named exports with descriptive function names
grep -r "export async function" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" .

# Locate the project's type checking configuration and run type validation
# Confirm async function signatures match expected contract types
# (Run the project's configured type checker)

# Identify the project's testing framework and execute test suites
# Covering async function exports to verify error handling, cache coordination, and state mutation behavior
# (Run the project's configured test suite)
```

**Accept when:**
- All public API contracts performing server-side operations are exported as async functions with named exports.
- Type checking passes without errors for all async function signatures and their usage sites.
- Test suites covering async exports demonstrate proper error handling, cache invalidation, and state coordination.
- Async function exports follow established naming conventions (imperative verbs for actions, descriptive nouns for page components, framework conventions for metadata generators).

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools MUST scan module exports to identify async functions and verify named export patterns. Code review MUST check that new async function exports follow naming conventions and implement consistent error handling. Type checking in continuous integration MUST validate async function signatures against expected contract types.
</enforcement>