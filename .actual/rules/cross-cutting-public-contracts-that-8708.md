# Export Async Server Functions as Named Public Contracts: Public Contracts That

These rules are ALWAYS ACTIVE for all server-side modules, page components, metadata generators, layout components, and API route handlers that perform server-side operations and coordinate async I/O, state mutations, and rendering.

### Rules

- **R-ASYNC-001** MUST: All public API contracts that perform server-side operations SHALL be exported as async functions with descriptive names reflecting their intent.
- **R-ASYNC-002** MUST: Server action modules that mutate application state SHALL use imperative verb naming conventions for async exports (e.g., `addItem`, `removeItem`, `updateItemQuantity`).
- **R-ASYNC-003** MUST: Page components that perform server-side data fetching SHALL use descriptive noun naming conventions matching route structure for async exports.
- **R-ASYNC-004** MUST: Metadata generation functions SHALL follow framework-mandated function names and async export patterns.
- **R-ASYNC-005** MUST: Layout components that coordinate async data loading SHALL export async functions with names reflecting their coordination purpose.
- **R-ASYNC-006** MUST: API route handlers that interact with external services SHALL be exported as async functions.
- **R-ASYNC-007** MUST: Async function exports SHALL implement consistent error handling including try-catch blocks for external service calls, contextual logging, user-facing error messages, and cache invalidation on failure.
- **R-ASYNC-008** MUST: Public async function exports SHALL delegate to internal helper functions for data access, transformation, and external service coordination to separate public contract from implementation.
- **R-ASYNC-009** SHOULD: Use named exports instead of default exports for all public async contracts to establish explicit contract surfaces and support multiple public functions per module.
- **R-ASYNC-010** MAY: Document exceptions to async export patterns with architectural review rationale and migration timelines for legacy modules.

### Verify

```bash
# Discover the project's module system configuration and identify all files exporting async functions
find . -type f \( -name '*.ts' -o -name '*.tsx' -o -name '*.js' -o -name '*.jsx' \) | xargs grep -l 'export.*async' | head -20

# Verify that public contracts use named exports with descriptive function names
grep -r 'export async function' . --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' | grep -v node_modules

# Locate the project's type checking configuration and run type validation
find . -maxdepth 2 -name 'tsconfig.json' -o -name 'jsconfig.json' | head -1

# Identify the project's testing framework and list test files
find . -type f \( -name '*.test.ts' -o -name '*.test.tsx' -o -name '*.spec.ts' -o -name '*.spec.tsx' \) | head -10

# Verify error handling patterns in async exports
grep -r 'try.*catch' . --include='*.ts' --include='*.tsx' --include='*.js' --include='*.jsx' -A 2 | grep -E '(export async|catch)' | head -20
```

**Accept when:**
- All public API contracts performing server-side operations are exported as async functions with named exports.
- Async function exports use descriptive names reflecting their intent (imperative verbs for actions, descriptive nouns for page components, framework-mandated names for metadata generators).
- Type checking passes without errors for all async function signatures and their usage sites.
- Test suites covering async exports demonstrate proper error handling, cache invalidation, and state coordination.
- Error handling is consistently implemented within async exports with logging, user-facing messages, and graceful degradation.
- Public async function exports delegate to internal helper functions for data access and transformation.

<enforcement>
Claude Code MUST NOT skip or defer verification. All async function exports MUST be validated against these rules before code is committed. Type checking and test execution are mandatory verification steps.
</enforcement>