# Export Async Server Functions as Named Public Contracts: Async Function Exports

These rules are ALWAYS ACTIVE for all server action modules, page components, metadata generation functions, layout components, and API route handlers that perform server-side operations and coordinate async I/O, state mutations, and rendering.

### Rules

- **R-ASYNC-001** MUST: Export async functions as named public contracts (not default exports) to establish explicit contract surfaces and enable multiple public functions per module.
- **R-ASYNC-002** MUST: Use imperative verb naming for server action exports that mutate application state (e.g., `addItem`, `removeItem`, `updateItemQuantity`).
- **R-ASYNC-003** MUST: Use descriptive noun naming for page component and layout exports that match route structure and coordinate async data loading.
- **R-ASYNC-004** MUST: Follow framework-mandated function names for metadata generation functions (e.g., `generateMetadata`).
- **R-ASYNC-005** MUST: Implement consistent error handling within async exports: wrap external service calls in try-catch blocks, log errors with context, return user-facing error messages or fallback values.
- **R-ASYNC-006** MUST: Separate public async function exports from internal implementation by delegating to helper functions for data access, transformation, and external service coordination.
- **R-ASYNC-007** MAY: Async function exports MAY accept Promise parameters to enable parallel data fetching and avoid waterfall request patterns.
- **R-ASYNC-008** SHOULD: Coordinate cache invalidation on async function failure to maintain consistency between server state and client-side caches.

### Verify

```bash
# Discover the project's module system configuration and identify all files exporting async functions
find . -type f \( -name '*.ts' -o -name '*.tsx' -o -name '*.js' -o -name '*.jsx' \) | xargs grep -l 'export async function' | head -20

# Verify that public contracts use named exports with descriptive function names
grep -r 'export async function' . --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' | grep -v 'node_modules'

# Locate the project's type checking configuration and run type validation
find . -maxdepth 2 -name 'tsconfig.json' -o -name 'jsconfig.json'

# Identify the project's testing framework and locate test files for async exports
find . -type f \( -name '*.test.ts' -o -name '*.test.tsx' -o -name '*.spec.ts' -o -name '*.spec.tsx' \) | head -10
```

**Accept when:**
- All public API contracts performing server-side operations are exported as async functions with named exports.
- Async function exports follow naming conventions: imperative verbs for actions, descriptive nouns for page/layout components, framework-mandated names for metadata generators.
- Type checking passes without errors for all async function signatures and their usage sites.
- Test suites covering async exports demonstrate proper error handling, cache invalidation, and state coordination.
- Error handling is consistently implemented within async exports with logging, user-facing messages, and graceful degradation.
- Public async function exports are separated from internal implementation through delegation to helper functions.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools MUST scan module exports to identify async functions and verify named export patterns. Code review MUST check that new async function exports follow naming conventions and implement consistent error handling. Type checking in continuous integration MUST validate async function signatures. Violations block pull request merges until patterns are corrected.
</enforcement>