# Client Component Directive ("use client") in React: Encapsulate Client Side Logic State Within

These rules are ALWAYS ACTIVE for React components that require client-side execution, interactivity, or state management, including those using `useState`, `useEffect`, or browser-specific APIs.

### Rules

- **R-REACT-CLIENT-001** SHOULD: Encapsulate client-side logic and state within components marked with "use client".

### Verify

```bash
# Discover the project's build command and run it to ensure no client/server component boundary errors are reported.
# Discover the project's test command and execute it to confirm client component functionality.
# Discover the project's linter command and run it to check for proper directive placement.
```

**Accept when:**
- The build process completes without errors related to client/server component boundaries.
- All client-side interactive features function as expected in the browser.
- Linter checks pass, ensuring correct "use client" directive usage.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>