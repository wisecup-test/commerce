# Client-Side React and Next.js Navigation Core Library Adoption: Access Url Search Parameters Within Client

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NAV-001** MUST: Access to URL search parameters within client components MUST be performed using `next/navigation`'s `searchParams.get()` function.

### Verify

```bash
# Discover the project's build command and execute it.
# Discover the project's test command and execute it.
# Discover the project's lint command and execute it.
```

**Accept when:**
- The build process completes without errors.
- All automated tests pass successfully.
- Linting reports no critical issues or warnings related to client-side component usage.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>