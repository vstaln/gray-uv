<p align="center">
  <img src="assets/gray-logo.svg" alt="gray" width="96">
  <img src="assets/uv.svg" alt="uv" width="96">
</p>
<h1 align="center">gray-uv</h1>
<p align="center">Rewrite Python packaging commands to their uv equivalents.</p>
<p align="center">
  <a href="https://github.com/vstaln/gray-uv/blob/main/LICENSE"><img alt="MIT License" src="https://img.shields.io/badge/license-MIT-blue.svg"></a>
  <img alt="gray plugin" src="https://img.shields.io/badge/gray-plugin-7aa2f7.svg">
  <img alt="rust" src="https://img.shields.io/badge/built%20with-rust-orange.svg">
</p>

Rewrite Python packaging commands to their uv equivalents — where a guard
would block `pip`/`poetry`, this rewrites it to `uv` instead.

## Commands

- `/uv on` / `/uv off` — toggle, persisted in `~/.gray/uv/enabled` (default on)
- `/uv status` — show state

## Wire

`plugin/manifest`, `tool/before`, `command/run`, `plugin/shutdown`.
Protocol 2.0, hook `tool/before`. No capabilities required.

## Install

```sh
gray plugin install uv
```

## Develop

```sh
cargo test
gray account check      # entry point + manifest handshake
gray account publish    # check → build → release → publish to the gray registry
```

Bump `version` in `Cargo.toml` before each `publish`; the registry refuses to
republish a version.

---
Part of the [gray](https://github.com/vstaln/gray) plugin ecosystem —
the open-source AI agent harness. <https://gray.alignment.id>
