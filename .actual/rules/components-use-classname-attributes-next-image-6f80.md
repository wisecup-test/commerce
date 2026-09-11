# Adopt `next/image` for Image Optimization: Use Classname Attributes Next Image Styling

These rules are ALWAYS ACTIVE for all UI components that display images.

### Rules

- **R-NIM-001** MAY: Use `className` attributes with `next/image` for styling and layout, potentially in conjunction with `clsx` for conditional class names.

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