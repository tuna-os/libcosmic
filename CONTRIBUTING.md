# Contributing to libcosmic

Thanks for your interest in contributing to libcosmic! This guide walks you through setting up your development environment, running tests, and submitting changes.

## Getting Started

### Prerequisites

libcosmic is written in Rust. You'll need:

- **Rust toolchain**: Install from [rustup.rs](https://rustup.rs/). libcosmic requires Rust 1.93 or later.
- **System libraries**: On Debian/Ubuntu-based systems (including Pop!_OS):
  ```bash
  sudo apt install cargo cmake just libexpat1-dev libfontconfig-dev libfreetype-dev libxkbcommon-dev pkgconf
  ```
- **just**: A command runner. Install via `cargo install just` or your package manager.

### Clone and Build

```bash
git clone --recurse-submodules https://github.com/tuna-os/libcosmic.git
cd libcosmic
just check-winit
```

The `--recurse-submodules` flag fetches iced and other embedded dependencies. If you forget this when cloning, run:

```bash
git submodule update --init --recursive
```

## Development Workflow

### Running Examples

libcosmic includes examples for application patterns. Run them with:

```bash
just run application
just run applet
just run calendar
just run config
# See justfile for the full list
```

Examples are in the `examples/` directory. Use them as templates for your own COSMIC projects.

### Checking Your Changes

Before committing, run the linter suite to catch errors and style issues:

```bash
# Check winit backend (most common for development)
just check-winit

# Check wayland backend
just check-wayland

# Check all examples
just check-examples

# Check everything at once
just check
```

If a check fails, read the output carefully — clippy messages usually suggest a fix.

### Generating Documentation

To build and view API documentation locally:

```bash
just doc
```

The generated docs appear in `target/doc/`. Open them in your browser to verify your docs render correctly:

```bash
open target/doc/cosmic/index.html  # macOS
xdg-open target/doc/cosmic/index.html  # Linux
```

### Building for Release

To test a release build (optimized, slower to compile):

```bash
cargo build --release
```

## Code Standards

- **Rust Edition**: The project uses Rust 2024 edition.
- **Clippy**: All code must pass `cargo clippy` with the flags in `justfile`. We enforce `clippy::all` and `clippy::pedantic`.
- **Formatting**: Use `cargo fmt` to format code before committing.
- **Dependencies**: When adding a new dependency, prefer features that integrate cleanly with iced and avoid duplicating async runtimes (choose tokio or smol, not both).

## Submitting Changes

### Before You Push

1. Run `just check` to verify your changes pass all checks.
2. Run `just doc` if you added or modified public API.
3. Include a clear commit message that explains the "why" behind your change.
4. Sign your commits with DCO: `git commit -s`.

### Creating a Pull Request

1. Push your branch: `git push -u origin guide/your-branch-name`
2. Open a PR on GitHub. Link any related issues.
3. Fill in the PR template if one appears.

The CI suite (same checks you ran locally) will run automatically. If CI fails:
- Review the error in the GitHub check details.
- Make fixes and push again — no need to close and reopen the PR.

### PR Guidelines

- **Scope**: Keep PRs focused. One feature or fix per PR when possible.
- **Commits**: Use clear commit messages. If your PR fixes an issue, mention it: `Fixes #123`.
- **Tests**: If you add new functionality, include examples showing how to use it.
- **Docs**: Update inline docs if your change affects the public API.

## Cargo Features

libcosmic supports several cargo features. When choosing which to enable during development:

- **`winit`** (default): Cross-platform windowing with X11 and Wayland support. Good for most development.
- **`wayland`** (default): Wayland client support.
- **`tokio`** (default): Async runtime. Use this unless you have a reason for `smol`.
- **`xdg-portal`**: XDG portal integrations for dialogs. Enable if testing portal integration.
- **`a11y`**: Accessibility support (experimental). Enable if working on a11y features.

Example: to build without wayland support:

```bash
cargo build --no-default-features --features=winit,tokio
```

## Project Templates

If you're building a COSMIC application or applet, use the official templates:

- [cosmic-app-template](https://github.com/pop-os/cosmic-app-template): Start here for new applications.
- [cosmic-applet-template](https://github.com/pop-os/cosmic-applet-template): Start here for panel applets.

These templates show best practices and include basic CI setup.

## Getting Help

- **Chat**: Mattermost at [chat.pop-os.org](https://chat.pop-os.org/)
- **Discussion**: GitHub issues and pull request comments
- **Design Questions**: Ask in chat before starting large refactors
- **Other Platforms**: Lemmy, Reddit, Mastodon (see README for links)

## Code of Conduct

Be respectful, inclusive, and professional. See the repository's CODE_OF_CONDUCT if one exists.

## Recognition

Contributors are recognized in the project. Thank you for improving libcosmic!
