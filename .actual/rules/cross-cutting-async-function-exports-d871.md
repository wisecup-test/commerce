# Export Async Server Functions as Named Public Contracts: Async Function Exports

These rules are ALWAYS ACTIVE for server action modules, page components, metadata generation functions, layout components, and API route handlers that coordinate async I/O, state mutations, and rendering.

### Rules

- **R-ASYNC-001** MUST: Async function exports SHALL use named exports rather than default exports to establish explicit contract boundaries.
- **R-ASYNC-002** MUST: Server actions use imperative verbs describing state mutations (e.g., `addItem`, `removeItem`, `updateItemQuantity`).
- **R-ASYNC-003** MUST: Page components use descriptive nouns matching route structure (e.g., `SearchPage`, `RootLayout`).
- **R-ASYNC-004** MUST: Metadata generators follow framework-mandated function names (e.g., `metadata`).
- **R-ASYNC-005** MUST: Implement consistent error handling within async exports: wrap external service calls in try-catch blocks, log errors with context, return user-facing error messages or fallback values.
- **R-ASYNC-006** MUST: Separate public async function exports from internal implementation by delegating to helper functions for data access, transformation, and external service coordination.
- **R-ASYNC-007** SHOULD: Coordinate cache invalidation on async function failure to maintain consistency.

### Verify

```bash
# Discover the project's module system configuration and identify all files exporting async functions
find . -type f \( -name '*.ts' -o -name '*.tsx' -o -name '*.js' -o -name '*.jsx' \) | xargs grep -l 'export async function' | head -20

# Verify that public contracts use named exports with descriptive function names
grep -r 'export async function' . --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' | grep -v 'export default'

# Locate the project's type checking configuration and run type validation
find . -name 'tsconfig.json' -o -name 'jsconfig.json' | head -1

# Identify the project's testing framework and execute test suites covering async function exports
find . -name 'package.json' | xargs grep -l '"test"' | head -1
```

**Accept when:**
- All public API contracts performing server-side operations are exported as async functions with named exports.
- Async function exports follow naming conventions: imperative verbs for actions, descriptive nouns for page components, framework-mandated names for metadata generators.
- Type checking passes without errors for all async function signatures and their usage sites.
- Test suites covering async exports demonstrate proper error handling, cache invalidation, and state coordination.
- Error handling is consistently implemented within async exports with logging, user-facing messages, and graceful degradation.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools MUST scan module exports to identify async functions and verify named export patterns. Code review MUST check that new async function exports follow naming conventions and implement consistent error handling. Type checking in continuous integration MUST validate async function signatures against expected contract types. Violations block pull request merges until patterns are corrected.
</enforcement>