# Contributing to LIBCOSMIC

Thanks for wanting to contribute! LIBCOSMIC is a platform toolkit for COSMIC desktop applications.

## Getting started

1. **Read AGENTS.md** — it documents the build, test, and development setup.
2. **Pick an issue or feature** — search the issue tracker for `good first issue` or discuss your idea in an issue first.
3. **Check the repo's `justfile`** — most common tasks are scripted there (`just check`, `just run <example>`).
4. **Sign your commits** — every commit must be DCO-signed (`git commit -s`).

## PR checklist

- [ ] Commit messages are signed off (`git commit -s`)
- [ ] `just check` passes (all clippy checks across feature combinations)
- [ ] `cargo test --all` passes
- [ ] Changes to public APIs include documentation comments
- [ ] Examples are updated if you change core components
- [ ] No secrets or machine-specific paths are committed

## Testing locally

Run the comprehensive checks before opening a PR:

```bash
just check      # Linting across wayland, winit, and examples
cargo test --all
```

To test your changes in an example application:

```bash
just run application
```

## Architecture notes

- **Feature flags control platform support.** Wayland and winit are mutually exclusive, as are tokio and smol. The justfile tests the primary combinations.
- **Examples are the integration test suite.** If you modify core components, verify the examples still build and run.
- **Accessibility is first-class.** When adding UI components, consider the accessibility implications.

## Getting help

- Ask in the relevant issue or PR.
- Join the community on [Mattermost](https://chat.pop-os.org/) or [Lemmy](https://lemmy.world/c/pop_os).
- See the [COSMIC App Template](https://github.com/pop-os/cosmic-app-template) for example usage.
