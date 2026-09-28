# Contributing to libcosmic

libcosmic is a platform toolkit for building COSMIC desktop applications and applets in Rust with iced. This guide covers contributions to the core library, examples, and documentation.

See [tuna-os/.github/CODE_OF_CONDUCT.md](https://github.com/tuna-os/.github/blob/main/CODE_OF_CONDUCT.md) for community guidelines.

## Set Up

You need Rust stable and system libraries. Installation varies by distribution:

**Fedora / RHEL:**
```bash
sudo dnf install cmake just libexpat1-devel libfontconfig-devel libfreetype-devel libxkbcommon-devel pkgconf
```

**Debian / Ubuntu:**
```bash
sudo apt-get install cmake just libexpat-dev libfontconfig-dev libfreetype-dev libxkbcommon-dev pkg-config
```

**macOS:**
```bash
brew install cmake just expat fontconfig freetype libxkbcommon pkg-config
```

Clone and build:
```bash
git clone https://github.com/tuna-os/libcosmic
cd libcosmic
cargo build
```

## Build and Test

```bash
# Build the library
cargo build

# Run tests
cargo test --lib

# Run all examples (requires Wayland or X11 display)
cargo build --examples
just run application          # or: animation, button_focus, etc.

# Check code with clippy
cargo clippy --all-targets
```

Examples are in `examples/`. Each demonstrates a feature or pattern:
- `application`: basic window and layout
- `animation`: animated state transitions
- `button_focus`: keyboard navigation
- `iced_todos`: a complete todo app (from iced upstream)

## Cargo Features

- `a11y`: Experimental accessibility support
- `animated-image`: Animated image support from the `image` crate
- `debug`: Additional debugging features
- `smol`: Use smol async runtime (conflicts with `tokio`)
- `tokio`: Use tokio async runtime (conflicts with `smol`)
- `wayland`: Wayland client windows (conflicts with `winit`)
- `winit`: X11 and cross-platform support (conflicts with `wayland`)
- `wgpu`: GPU-accelerated rendering (default: softbuffer for software rendering)
- `xdg-portal`: XDG portal dialog integrations

To test a feature combination:
```bash
cargo build --features wayland,wgpu,a11y
cargo test --lib --features tokio,winit
```

## Code Conventions

- Follow Rust style via `cargo fmt`
- Use clippy to catch common mistakes: `cargo clippy --all-targets`
- Module layout: API surface at the crate root, impl details in modules
- Widget patterns are in `cosmic::widget::*` — follow their structure
- Theme and styling: use the COSMIC theme colors and spacing

## Pull Requests

1. Open or find an issue for your change
2. Branch from `master`: `git checkout -b feature/your-feature`
3. Write commit messages: `feat: …`, `fix: …`, `refactor: …`, `docs: …`
4. Build and test locally: `cargo test --lib && cargo clippy --all-targets`
5. Run an example that exercises your change
6. Push and open a PR against `master`

For breaking API changes, file an issue first to discuss the change.

## Documentation

- [API Documentation](https://pop-os.github.io/libcosmic/cosmic/): Generated from inline docs
- [libcosmic Book](https://pop-os.github.io/libcosmic-book/): Learning guide and patterns
- [cosmic-app-template](https://github.com/pop-os/cosmic-app-template): Application starter
- [cosmic-applet-template](https://github.com/pop-os/cosmic-applet-template): Applet starter

Add doc comments to public items: `///` for items, `//!` for modules. Examples in doc comments help users understand API usage.

## Submitting Ideas

- **Feature proposals**: Open an issue with use case and design sketch
- **API feedback**: Try building something with the current API first, then file an issue with what felt awkward
- **Examples**: Submit new examples as PRs — they help other developers learn

---

By contributing, you agree your contributions are licensed under the Mozilla Public License 2.0.
