# Export Async Server Functions as Named Public Contracts: Server Action Functions

These rules are ALWAYS ACTIVE for all server action modules, page components, metadata generation functions, layout components, and API route handlers that interact with external services or perform server-side data fetching and state mutations.

### Rules

- **R-ASYNC-001** MUST: Server action functions that mutate state SHALL be exported as async functions and coordinate cache invalidation, cookie management, and error handling within their scope.

### Verify

```bash
# Discover the project's module system configuration and identify all files exporting async functions
# Verify that public contracts use named exports with descriptive function names
find . -type f \( -name '*.ts' -o -name '*.tsx' -o -name '*.js' -o -name '*.jsx' \) | xargs grep -l 'export async function' | head -20

# Locate the project's type checking configuration and run type validation
# Confirm async function signatures match expected contract types
npm run type-check 2>&1 | grep -E '(error|Error)' || echo 'Type checking passed'

# Identify the project's testing framework and execute test suites
# Verify error handling, cache coordination, and state mutation behavior
npm run test -- --testPathPattern='(action|server|metadata)' 2>&1 | tail -20
```

**Accept when:**
- All public API contracts performing server-side operations are exported as async functions with named exports.
- Type checking passes without errors for all async function signatures and their usage sites.
- Test suites covering async exports demonstrate proper error handling, cache invalidation, and state coordination.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis tools MUST scan module exports to identify async functions and verify named export patterns. Code review MUST check that new async function exports follow naming conventions and implement consistent error handling. Type checking in continuous integration MUST validate async function signatures against expected contract types. Violations block pull request merges until patterns are corrected.
</enforcement>