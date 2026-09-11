# Client Component Directive ("use client") in React: Ensure That Components Marked Use Client

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-CLIENT-COMP-001** MUST: Ensure that components marked with "use client" are only imported by other client components or passed as props from server components.

### Verify

```bash
# Discover the project's build command and run it to ensure no client/server component boundary errors are reported.
# Example: npm run build or yarn build
$(discover_build_command)

# Discover the project's test command and execute it to confirm client component functionality.
# Example: npm test or yarn test
$(discover_test_command)

# Discover the project's linter command and run it to check for proper directive placement.
# Example: npm run lint or yarn lint
$(discover_linter_command)
```

**Accept when:**
- The build process completes without errors related to client/server component boundaries.
- All client-side interactive features function as expected in the browser.
- Linter checks pass, ensuring correct "use client" directive usage.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>