# Export Async Server Functions as Named Public Contracts: Page Component Exports

These rules are ALWAYS ACTIVE for server action modules, page components, metadata generation functions, layout components, and API route handlers that perform server-side data fetching, state mutations, or coordinate async I/O operations.

### Rules

- **R-ASYNC-001** SHOULD: Page component exports that require data fetching SHOULD implement async function signatures to enable server-side rendering and metadata generation.
- **R-ASYNC-002** SHOULD: Server action modules that mutate application state SHOULD export async functions as named public contracts with descriptive function names.
- **R-ASYNC-003** SHOULD: Metadata generation functions SHOULD use async function signatures following framework-mandated naming conventions.
- **R-ASYNC-004** SHOULD: Layout components that coordinate async data loading SHOULD export async functions as named public contracts.
- **R-ASYNC-005** SHOULD: API route handlers that interact with external services SHOULD implement async function signatures.
- **R-ASYNC-006** SHOULD: Async exports SHOULD use named exports rather than default exports to establish explicit contract surfaces.
- **R-ASYNC-007** SHOULD: Server actions SHOULD use imperative verb naming conventions describing state mutations (e.g., addItem, removeItem, updateItemQuantity).
- **R-ASYNC-008** SHOULD: Page components SHOULD use descriptive noun naming conventions matching route structure.
- **R-ASYNC-009** SHOULD: Async function exports SHOULD delegate to helper functions for data access, transformation, and external service coordination to separate public contracts from internal implementation.
- **R-ASYNC-010** SHOULD: Async exports SHOULD implement consistent error handling with try-catch blocks, logging, user-facing error messages, and graceful degradation strategies.

### Verify

```bash
# Discover the project's module system configuration and identify all files exporting async functions
find . -type f \( -name '*.ts' -o -name '*.tsx' -o -name '*.js' -o -name '*.jsx' \) | xargs grep -l 'export.*async' | head -20

# Verify that public contracts use named exports with descriptive function names
grep -r 'export async function' . --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' | grep -v node_modules

# Locate the project's type checking configuration and run type validation
find . -maxdepth 2 -name 'tsconfig.json' -o -name 'jsconfig.json' | head -1

# Identify the project's testing framework and verify test coverage for async exports
find . -maxdepth 2 \( -name 'jest.config.*' -o -name 'vitest.config.*' -o -name 'package.json' \) | head -1
```

**Accept when:**
- All public API contracts performing server-side operations are exported as async functions with named exports.
- Async function exports follow naming conventions: imperative verbs for server actions, descriptive nouns for page components, framework-mandated names for metadata generators.
- Type checking passes without errors for all async function signatures and their usage sites.
- Test suites covering async exports demonstrate proper error handling, cache invalidation, and state coordination.
- Async function exports delegate to helper functions, separating public contracts from internal implementation.
- Error handling is consistently implemented within async exports with logging and user-facing error messages.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools MUST scan module exports to identify async functions and verify named export patterns. Code review MUST check that new async function exports follow naming conventions and implement consistent error handling. Type checking in continuous integration MUST validate async function signatures. Violations block pull request merges until patterns are corrected.
</enforcement>