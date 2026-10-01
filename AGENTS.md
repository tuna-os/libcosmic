# AGENTS.md — agent guide for tuna-os/libcosmic

**libcosmic** is a platform toolkit based on iced for creating COSMIC desktop applications and applets.

Human docs: [`README.md`](README.md) (overview and links),
[`CONTRIBUTING.md`](CONTRIBUTING.md) (development setup and PR process).

## Build and Test

```bash
just check-winit     # lint and check winit backend
just check-wayland   # lint and check wayland backend
just check           # full check suite (both backends + examples)
just doc             # generate API documentation
just run <example>   # run an example (see justfile for names)
```

The `justfile` is the source of truth for all build and check commands.

## Key Files

- **src/**: Main libcosmic library code
- **examples/**: Application and applet templates for contributors
- **Cargo.toml**: Package manifest with feature definitions
- **cosmic-config/**, **cosmic-config-derive/**: Derived configuration macros
- **cosmic-theme/**: Theme and styling system

## Dependencies

Build prerequisites vary by platform. See [`CONTRIBUTING.md`](CONTRIBUTING.md#prerequisites) for installation instructions.

Key features:
- **winit** / **wayland**: Windowing (choose one)
- **tokio** / **smol**: Async runtime (choose one)
- **xdg-portal**: XDG portal integrations
- **a11y**: Experimental accessibility

## Cargo Features

Feature selection affects compilation:

```bash
cargo build --no-default-features --features=winit,tokio
cargo build --features=wayland,xdg-portal
```

See `Cargo.toml` for the full feature matrix.

## Known Constraints

- **Rust 1.93+** required (see `Cargo.toml` `rust-version`)
- **Submodules**: Clone with `--recurse-submodules` to fetch iced and other dependencies
- **Feature conflicts**: winit/wayland and tokio/smol are mutually exclusive

## Project Links

- **API Docs**: https://pop-os.github.io/libcosmic/cosmic/ (auto-generated from source)
- **libcosmic Book**: https://pop-os.github.io/libcosmic-book/ (learning reference)
- **Templates**: cosmic-app-template, cosmic-applet-template on GitHub
- **Made-for-COSMIC IDs**: Use `com.system76.CosmicApplication` or `com.system76.CosmicApplet` in metainfo

## Examples

The `examples/` directory includes starter projects:
- `application`: Full desktop application
- `applet`: Panel applet
- `calendar`, `config`, `menu`: Pattern examples

Run them with `just run <name>` or `cargo run --release -p <name>`.

## DCO and Attribution

All commits must be signed with DCO: `git commit -s`. No special PR attribution needed beyond the standard Git author.
