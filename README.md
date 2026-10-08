# gray-uv

Rewrite Python packaging commands to their uv equivalents.

A sidecar plugin for [gray](https://github.com/vstaln/gray). Port-by-spec of
mitsupi's `uv.ts`/`intercepted-commands/` (proprietary — reimplemented, no
code copied): where the original blocked pip/poetry, this rewrites.

`tool/before` on `bash` inspects the first shell segment of `args.command`
and answers `{"decision":"modify"}`:

| typed                  | runs                    |
|------------------------|-------------------------|
| `pip install X`        | `uv pip install X`      |
| `pip3 …`               | `uv pip …`              |
| `poetry add X`         | `uv add X`              |
| `poetry install`       | `uv sync`               |
| `poetry remove X`      | `uv remove X`           |
| `poetry run C`         | `uv run C`              |
| `python -m venv …`     | `uv venv …`             |

Only the first segment (before `|` `;` `&` or a newline) is examined, and
only when the tool word is its first token — `echo x | pip install y` and
`sudo pip install y` pass through untouched. Everything after the first
separator is preserved verbatim. Fail open: any rewrite problem → allow.

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
