# hubris-userlib

A filtered subset of [oxidecomputer/hubris](https://github.com/oxidecomputer/hubris)
containing the `userlib` crate and its (transitive) path dependencies, packaged
as a standalone Cargo workspace so it can be easily vendored into another
project.

The git history for every included crate is preserved (extracted with
`git filter-repo`). Licensed under MPL-2.0; see [`LICENSE.txt`](LICENSE.txt).

## Layout

| crate | path | role |
| --- | --- | --- |
| `userlib` | `sys/userlib` | the crate of interest |
| `abi` | `sys/abi` | kernel/user ABI definitions |
| `armv6m-atomic-hack` | `lib/armv6m-atomic-hack` | atomic shims for ARMv6-M |
| `unwrap-lite` | `lib/unwrap-lite` | small `unwrap` helper |
| `volatile-const` | `lib/volatile-const` | volatile-const access helper |
| `phash` | `lib/phash` | perfect-hash support (used by `abi`) |
| `toml-task` | `lib/toml-task` | TOML task schema (used by `build-util`) |
| `build-util` | `build/util` | build-script helpers |

`userlib` is `#![no_std]` and only builds for ARM thumb targets. To check it:

```sh
cargo check -p userlib --target thumbv7em-none-eabihf
```

The host-buildable support crates (`abi`, `phash`, `toml-task`, `build-util`,
etc.) build under `cargo check --workspace`.

## Workspace dependencies

Common dependency versions and lints are defined once in the top-level
[`Cargo.toml`](Cargo.toml) under `[workspace.dependencies]` /
`[workspace.lints]`, and the member crates reference them with
`{ workspace = true }`, exactly as upstream. Only the subset of upstream
workspace dependencies actually used by these crates is included.
