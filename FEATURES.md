# COSMIC Toolkit Features

This document describes libcosmic's feature flags, their dependencies, and usage guidance.

## Feature Tiers

Features are organized by purpose and dependency requirements.

### Tier 1: Rendering & Window Management (Required for any desktop app)

| Feature | Purpose | Dependencies | Conflicts | Default |
|---------|---------|--------------|-----------|---------|
| `winit` | Native window management via winit | — | — | ✓ |
| `wgpu` | GPU rendering via wgpu | `iced/wgpu`, `iced_wgpu` | — | ✗ |
| `x11` | X11 display server support | `iced/x11`, `iced_winit/x11` | — | ✓ |
| `wayland` | Wayland display server support | `autosize`, `surface-message`, `cctk`, `multi-window`, `winit` | — | ✓ |

**Guidance**: Pick at least one of `winit` + (`wgpu` or default tiny-skia) for window rendering. Include `x11` and/or `wayland` for the target platform.

### Tier 2: Async Runtime (Required)

| Feature | Purpose | Dependencies | Conflicts | Default |
|---------|---------|--------------|-----------|---------|
| `tokio` | Tokio async runtime (recommended) | `dep:tokio`, propagates to dependencies | `smol`, `async-std` | ✓ |
| `smol` | Smol async runtime | `dep:smol`, `iced/smol`, `zbus?/async-io`, `rfd?/async-std` | `tokio`, `async-std` | ✗ |
| `async-std` | Async-std runtime | `dep:async-std`, propagates to dependencies | `tokio`, `smol` | ✗ |

**Guidance**: Pick exactly one. Default is `tokio`. These are mutually exclusive — building with multiple will fail.

### Tier 3: Configuration & Desktop Integration

| Feature | Purpose | Dependencies | Conflicts | Default |
|---------|---------|--------------|-----------|---------|
| `dbus-config` | Cosmic settings daemon integration (Linux only) | `cctk`, `cosmic-settings-daemon`, `zbus` | — | ✓ |
| `desktop` | Desktop file and freedesktop support | `process`, `cosmic-settings-config`, `freedesktop-desktop-entry`, `image-extras`, `mime`, `shlex` | — | ✗ |
| `desktop-systemd-scope` | Launch desktop files in systemd scopes | `desktop`, `dep:zbus` | — | ✗ |
| `single-instance` | Prevent multiple app instances | `iced_winit/single-instance`, `zbus/blocking-api`, `ron` | — | ✗ |
| `process` | Process spawning utilities | `dep:libc`, `dep:rustix` | — | ✗ |

**Guidance**: `dbus-config` requires D-Bus (Linux only, conditional). `desktop` requires freedesktop ecosystem. Use `desktop-systemd-scope` only when launching via systemd is a requirement.

### Tier 4: Accessibility & UI Features

| Feature | Purpose | Dependencies | Conflicts | Default |
|---------|---------|--------------|-----------|---------|
| `a11y` | Accessibility support (screen readers, keyboard nav) | `iced/a11y`, `iced_accessibility` | — | ✓ |
| `about` | Built-in about widget | — | — | ✗ |
| `qr_code` | QR code rendering widget | `iced/qr_code` | — | ✗ |
| `markdown` | Markdown rendering support | `iced/markdown` | — | ✗ |
| `highlighter` | Syntax highlighting for text | `iced/highlighter` | — | ✗ |
| `advanced-shaping` | Advanced text shaping (complex scripts) | `iced/advanced-shaping` | — | ✗ |

**Guidance**: `a11y` is highly recommended. Enable optional widgets (`about`, `qr_code`, `markdown`) only if your app uses them to reduce binary size.

### Tier 5: Pattern Support

| Feature | Purpose | Dependencies | Conflicts | Default |
|---------|---------|--------------|-----------|---------|
| `applet` | COSMIC panel applet support | `autosize`, `winit`, `wayland`, `tokio`, `cosmic-panel-config`, `ron`, `multi-window` | — | ✗ |
| `applet-token` | Applet token/identity verification | `applet` | — | ✗ |
| `multi-window` | Multi-window application support | — | — | ✓ |
| `autosize` | Automatic window sizing | — | — | ✗ |

**Guidance**: `applet` requires specific platform features. Apps should not enable it unless they're building applets. `multi-window` enables multi-window support; omit only for single-window apps. `autosize` is internal; omit unless specifically needed.

### Tier 6: File Dialogs & Portals

| Feature | Purpose | Dependencies | Conflicts | Default |
|---------|---------|--------------|-----------|---------|
| `rfd` | RFD-based file dialogs | `dep:rfd` (feature-gated) | `xdg-portal` | ✗ |
| `xdg-portal` | XDG portal file/media access | `ashpd` | — | ✗ |
| `pipewire` | PipeWire integration (in ashpd) | `ashpd?/pipewire` | — | ✗ |

**Guidance**: For file dialogs, pick `rfd` (local) or `xdg-portal` (sandboxed/portal). Default (neither) uses platform defaults.

### Tier 7: Media & Image Support

| Feature | Purpose | Dependencies | Conflicts | Default |
|---------|---------|--------------|-----------|---------|
| `animated-image` | GIF, WebP, PNG animation | `async-fs`, `image/*` feature set, `tokio?/io-util`, `tokio?/fs` | — | ✗ |

**Guidance**: Enable only if your app displays animated images. Adds async I/O dependencies.

### Tier 8: Debug & Testing

| Feature | Purpose | Dependencies | Conflicts | Default |
|---------|---------|--------------|-----------|---------|
| `debug` | Debug rendering overlay | `iced/debug` | — | ✗ |
| `serde-keycode` | Serialize keycodes | `iced_core/serde` | — | ✗ |

**Guidance**: Use `debug` for development only. `serde-keycode` is for apps that need to persist keyboard shortcuts.

## Common Feature Combinations

### Standard Desktop App
```toml
libcosmic = { version = "1.0", default-features = true }
# Includes: winit, tokio, a11y, dbus-config, x11, wayland, multi-window
```

### Minimal App (size-optimized)
```toml
libcosmic = { version = "1.0", default-features = false, features = ["winit", "tokio", "x11"] }
```

### Applet
```toml
libcosmic = { version = "1.0", default-features = false, features = ["applet", "wayland", "tokio"] }
```

### Web Target (future)
```toml
libcosmic = { version = "1.0", default-features = false, features = ["web", "async-std"] }
```

### Server/Headless (no windowing)
```toml
# Not supported — libcosmic requires a windowing system
```

## CI Testing Matrix

The following combinations are tested in CI to catch regressions:

```bash
# Default features
cargo test --all

# Minimal
cargo test --no-default-features --features winit,tokio,x11

# Applet support
cargo test --no-default-features --features applet

# Desktop integration
cargo test --no-default-features --features desktop,winit,tokio

# Alternative runtime
cargo test --no-default-features --features winit,smol

# All features
cargo test --all-features
```

## Adding New Features

When adding a feature:

1. Determine its tier (see above)
2. Document dependencies explicitly in Cargo.toml comments
3. Document conflicts with other features in this file
4. Add to the CI matrix if the combination is not tested
5. Never introduce feature interdependencies without documenting why

## Known Limitations

- Cannot combine `tokio`, `smol`, and `async-std` in the same build
- `dbus-config` is Linux-only and has no effect on other platforms
- `applet` requires `wayland` and `tokio` (cannot use `applet + smol`)
- `rfd` and `xdg-portal` are mutually exclusive file dialog backends
