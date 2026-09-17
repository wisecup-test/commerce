# Client-Side React and Next.js Navigation Core Library Adoption: React Usememo Hook Used Memoizing Computationally

These rules are ALWAYS ACTIVE for all client-side React components, Next.js application pages, and modules importing `react` or `next/navigation` within the project.

### Rules

- **R-ADR-001** MUST: React's `useMemo` hook MUST be used for memoizing computationally expensive values within client components to optimize performance.

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