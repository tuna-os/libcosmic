# Agent Development Guide

A file for [guiding coding agents](https://agents.md/).

## What this repository is

LIBCOSMIC is a platform toolkit based on iced for creating applets and applications for the COSMIC desktop environment. It is a Rust library that provides UI components, theming, and layout primitives for COSMIC applications.

## Build and test

Build commands use the provided `justfile`:

```bash
just check           # Run all checks (clippy with wayland, winit, and examples)
just check-wayland   # Run clippy for wayland feature set
just check-winit     # Run clippy for winit feature set
just check-examples  # Run clippy for all examples
just doc             # Generate documentation (requires nightly-2026-04-27)
cargo test           # Run the test suite
```

The primary checks that gate PRs:

```bash
just check           # Comprehensive linting across all feature combinations
cargo test --all     # Unit tests
```

## Contributing rules that CI enforces

- Every commit carries a DCO sign-off: `git commit -s`.
- PRs target `master`.
- Run `just check` before pushing to verify all feature combinations pass clippy.
- Documentation is built with a specific nightly toolchain; see `justfile` for the exact version.

## Repository-specific notes

- **Feature combinations:** LIBCOSMIC supports multiple feature flags (`wayland`/`winit`, `tokio`/`smol`, `wgpu` for GPU rendering). The check recipes test the primary combinations.
- **Nightly toolchain:** Documentation generation requires `nightly-2026-04-27` for `rustdoc` configuration.
- **Examples:** The repository includes example projects (applet, application, calendar, etc.). Changes to core APIs should be validated against examples.
- **Accessibility:** The `a11y` feature provides experimental accessibility support and should be considered when adding UI components.

For issues and PRs, see [CONTRIBUTING.md](CONTRIBUTING.md). For API documentation and learning materials, see the [libcosmic Book](https://pop-os.github.io/libcosmic-book/).
