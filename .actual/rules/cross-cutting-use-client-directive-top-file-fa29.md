# Client Component Directive ("use client") in React: Use Client Directive Top File Designate

These rules are ALWAYS ACTIVE for React component files designated as client components.

### Rules

- **R-CLIENT-001** MUST: Use the "use client" directive at the top of a file to designate a component as a client component when it requires client-side interactivity, state, or browser APIs.

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