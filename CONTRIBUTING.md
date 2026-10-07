# Contributing to libcosmic

Thank you for your interest in contributing to libcosmic! This document explains how to get started, how to build the project, and how to submit pull requests.

## Quick Start

### Prerequisites

libcosmic is written in Rust and uses the Cargo build system. You'll need:

- **Rust toolchain**: Install from [rustup.rs](https://rustup.rs/)
- **System libraries**: Required for graphics, font rendering, and system integration

**On Pop!_OS:**
```sh
sudo apt install cargo cmake just libexpat1-dev libfontconfig-dev libfreetype-dev libxkbcommon-dev pkgconf
```

**On Ubuntu/Debian (generic):**
```sh
sudo apt install cargo cmake just libexpat1-dev libfontconfig-dev libfreetype-dev libxkbcommon-dev pkgconf
```

**On Fedora/RHEL:**
```sh
sudo dnf install gcc cmake cargo just expat-devel fontconfig-devel freetype-devel libxkbcommon-devel pkgconfig
```

**On Arch:**
```sh
sudo pacman -S base-devel cmake just expat fontconfig freetype2 libxkbcommon pkg-config
```

**On macOS:**
```sh
brew install cmake just
```

**On other systems:** Check if your package manager has the equivalents of: cmake, just, expat-devel, fontconfig-devel, freetype-devel, libxkbcommon-devel, pkg-config.

### Setting Up the Repository

```sh
# Clone with submodules (needed for examples and translations)
git clone --recurse-submodules https://github.com/tuna-os/libcosmic.git
cd libcosmic

# Or, if you already cloned without submodules:
git submodule update --init --recursive
```

### Building and Testing

```sh
# Build the library in release mode
cargo build --release

# Run the full test suite
cargo test --workspace

# Run formatting check
cargo fmt --all --check

# Run linter
cargo clippy --workspace --all-targets

# Check all at once (recommended before submitting PR)
just lint
```

The `just` tool runs predefined commands. Use `just --list` to see all available commands:

```sh
just --list
```

## Working with Examples

The `examples/` directory contains demonstrations of libcosmic features. Each example is a complete, runnable project.

### Running an Example

```sh
# List available examples
ls examples/

# Run an example (e.g., the application example)
just run application

# Run a specific example
just run calendar
```

### Example Structure

Each example directory contains:
- `Cargo.toml` — Project configuration
- `src/main.rs` — The example code

### Common Examples to Start With

- **`application`** — Basic application structure; start here as a template
- **`applet`** — How to create a COSMIC panel applet
- **`config`** — Using the configuration system
- **`open-dialog`** — File dialogs and file picking
- **`context-menu`** — Right-click context menus
- **`table-view`** — Data tables and list views

### Creating Your Own Example

1. Create a new directory in `examples/your-feature/`
2. Add a `Cargo.toml` (copy from an existing example and modify)
3. Add `src/main.rs` with your demonstration code
4. Document it in `examples/README.md`
5. Run it with `just run your-feature`

## Code Style and Conventions

### Formatting

libcosmic uses `rustfmt` for automatic formatting. Before committing:

```sh
cargo fmt --all
```

### Linting

Use `clippy` to catch common mistakes:

```sh
cargo clippy --workspace --all-targets -- -D warnings
```

### Naming and Organization

- **Modules**: Use descriptive names reflecting their responsibility (e.g., `widget/button.rs`)
- **Functions**: Use `snake_case` for function names
- **Types**: Use `PascalCase` for struct and trait names
- **Constants**: Use `SCREAMING_SNAKE_CASE` for constants

### Documentation Comments

Public items should include doc comments:

```rust
/// A brief description of what this does.
///
/// # Examples
///
/// ```
/// // Example usage
/// ```
pub fn my_function() {
    // implementation
}
```

### Comments

- **Why, not what**: Comments should explain *why* a design decision was made, not what the code does (the code shows that)
- **Real comments only**: Avoid comments that just repeat the code

```rust
// Good: explains the reason
// We use a separate task for this because the operation may block for several seconds
let result = expensive_operation().await;

// Avoid: just repeats the code
// increment counter
counter += 1;
```

## Project Structure

### Root-Level Organization

| Directory | Purpose |
|---|---|
| `src/` | Core library code (main libcosmic API) |
| `examples/` | Runnable example applications |
| `cosmic-config/` | Configuration management crate |
| `cosmic-config-derive/` | Derive macros for config |
| `cosmic-theme/` | Theme system and styling |
| `cosmic-icons/` | Icon theme integration |
| `i18n/` | Internationalization data |

### `src/` Organization

The main library code is organized by feature area:

| File/Module | Purpose |
|---|---|
| `lib.rs` | Main library exports and documentation |
| `widget/` | UI widgets (buttons, text input, calendar, etc.) |
| `theme/` | Theme and styling system |
| `app/` | Application framework and lifecycle |
| `core.rs` | Core types and utilities |
| `command.rs` | Command/message system for user interactions |
| `action.rs` | Action definitions for accessibility |
| `anim.rs` | Animation utilities |
| `icon_theme.rs` | Icon loading and theming |
| `desktop.rs` | Desktop integration (XDG, DBus, etc.) |

### Adding a New Widget

1. Create `src/widget/your_widget.rs`
2. Implement the widget's `view()`, `update()`, and `Message` enum
3. Export it in `src/widget/mod.rs`
4. Add it to `src/lib.rs` exports if it's public API
5. Create an example in `examples/your-widget/`
6. Add tests in the widget module's `#[cfg(test)]` section

## Features and Cargo Configuration

libcosmic has several optional features. Check `Cargo.toml` for the current list. When working on features:

- Test with the feature enabled: `cargo test --features your-feature`
- Test without the feature: `cargo test --no-default-features`
- Consider if your code should be gated behind a feature

## Submitting Changes

### Before You Start

1. Check if there's an open issue for the work you want to do
2. If not, open one and describe what you plan to do
3. For large changes, discussion in the issue first saves time

### Making Changes

1. **Create a branch** from `master`:
   ```sh
   git checkout -b descriptive-branch-name
   ```

2. **Make your changes**: Edit files, add features, fix bugs

3. **Test thoroughly**:
   ```sh
   cargo test --workspace
   cargo clippy --workspace --all-targets -- -D warnings
   cargo fmt --all --check
   ```

4. **Add a test** if you're fixing a bug or adding a feature:
   - Unit tests go in the same file as the code
   - Use `#[cfg(test)]` blocks
   - Make the test name descriptive

5. **Commit with a clear message**:
   ```sh
   git commit -am "Brief description of the change"
   ```
   - Keep the first line under 50 characters
   - Use imperative mood ("Add feature" not "Added feature")
   - Reference related issues if applicable (e.g., "Fixes #123")

### Opening a Pull Request

1. **Push your branch**:
   ```sh
   git push origin your-branch-name
   ```

2. **Open a PR** on GitHub with:
   - Clear title describing the change
   - Description explaining *what* changed and *why*
   - Reference to any related issues
   - Screenshots for UI changes

3. **Respond to feedback**: The maintainers may request changes. This is normal and collaborative.

### What Gets Reviewed

Reviewers will check:
- **Correctness**: Does it do what it claims?
- **Tests**: Are there tests? Do they pass?
- **Style**: Does it follow the project conventions?
- **Performance**: Does it introduce unnecessary overhead?
- **Accessibility**: Are there accessibility concerns?
- **Documentation**: Are public APIs documented?

## Getting Help

If you have questions:

- **Chat**: [Mattermost](https://chat.pop-os.org/) (COSMIC channel)
- **Issues**: Open a [GitHub issue](https://github.com/tuna-os/libcosmic/issues)
- **Documentation**: Check the [libcosmic Book](https://pop-os.github.io/libcosmic-book/)
- **API Docs**: Generated docs are at `cargo doc --open`

## Common Development Tasks

### Debugging

Enable debug logging by running with `RUST_LOG`:

```sh
RUST_LOG=debug just run application
```

### Viewing Generated Documentation

```sh
cargo doc --open
```

### Running a Specific Test

```sh
cargo test test_name -- --nocapture
```

### Building for a Specific Feature Set

```sh
cargo build --no-default-features --features wayland
cargo build --no-default-features --features x11
```

## License

By contributing to libcosmic, you agree that your contributions will be licensed under the [Mozilla Public License 2.0](https://choosealicense.com/licenses/mpl-2.0/), the same license as the project.

## Code of Conduct

Please be respectful and constructive in all interactions. We're all volunteers building tools for the COSMIC desktop.
