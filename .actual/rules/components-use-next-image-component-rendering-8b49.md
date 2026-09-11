# Adopt `next/image` for Image Optimization: Use Next Image Component Rendering Within

These rules are ALWAYS ACTIVE for all files matching the configured scope.

### Rules

- **R-NXTIMG-001** MUST: MUST use the `next/image` component for all image rendering within UI components.

### Verify

```bash
# Inspect the project's dependency manifest to confirm `next` is listed.
# Run the project's build command and observe image optimization logs.
# Execute the project's test suite to ensure no image-related regressions.
```

**Accept when:**
- `next/image` is the primary component used for displaying images in UI components.
- Images are optimized and served efficiently in production builds.
- No significant performance regressions are introduced by image handling.

<enforcement>
Claude Code MUST NOT skip or defer verification.
</enforcement>