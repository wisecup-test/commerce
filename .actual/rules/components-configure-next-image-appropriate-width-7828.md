# Adopt `next/image` for Image Optimization: Configure Next Image Appropriate Width Height

These rules are ALWAYS ACTIVE for all UI components that display images.

### Rules

- **R-NEXT-IMAGE-001** SHOULD: Configure `next/image` with appropriate `width`, `height`, `alt`, and `priority` properties for optimal performance and accessibility.

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