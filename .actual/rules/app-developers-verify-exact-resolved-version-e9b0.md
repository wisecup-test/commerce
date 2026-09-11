# Adopt Next.js Framework for Web Application Development: Developers Verify Exact Resolved Version Any

These rules are ALWAYS ACTIVE for all code related to web application development in this project.

### Rules

- **R-NEXTJS-001** MUST: Developers MUST verify the exact resolved version of any versioned dependency by inspecting the project's lock file before implementation.

### Verify

```bash
# As per ADR's DISCOVERY POLICY, specific tool names and commands are omitted.
# Consumer MUST derive them from the project repository.

# Inspect the project's dependency manifest to identify the declared framework.
# Execute the project's build command to confirm successful compilation.
# Run the project's test suite to validate core functionalities.
```

**Accept when:**
- The dependency manifest explicitly lists the framework.
- The build process completes without errors.
- All automated tests pass successfully.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>