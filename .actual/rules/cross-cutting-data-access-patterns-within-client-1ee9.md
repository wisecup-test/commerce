# Client-Side React and Next.js Navigation Core Library Adoption: Data Access Patterns Within Client Components

These rules are ALWAYS ACTIVE for client-side React components, Next.js application pages requiring client-side interactivity, and modules importing `react` or `next/navigation`.

### Rules

- **R-CLIENT-DATA-001** SHOULD: Data access patterns within client components, such as finding items in a collection, SHOULD use standard array methods like `find`.

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