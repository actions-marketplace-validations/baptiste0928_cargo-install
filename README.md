# Cargo install action

![GitHub release (latest by date)](https://img.shields.io/github/v/release/baptiste0928/cargo-install)
[![JavaScript Style Guide](https://img.shields.io/badge/code_style-standard-brightgreen.svg)](https://standardjs.com)
[![CI](https://github.com/baptiste0928/cargo-install/actions/workflows/ci.yml/badge.svg)](https://github.com/baptiste0928/cargo-install/actions/workflows/ci.yml)

This action enables you to run `cargo install` in your GitHub workflows,
and automatically caches the resulting binaries to speed up subsequent builds.

> ✨ **Update:** The recent v2 update introduces some breaking changes. Read
> the [changelog] before updating.

## Features
- Install any Rust binary crate published on [crates.io].
- Automatically cache installed binaries to avoid compiling them each run.
- Keep crates updated, with an optional version range to avoid breakages.
- Works on Linux, Windows and MacOS runners.

## Usage
The following example steps install the [`cargo-hack`] crate. Read
[Quickstart for GitHub Actions] to learn more about Actions usage.

```yaml
- name: Install cargo-hack
  uses: baptiste0928/cargo-install@v2
  with:
    crate: cargo-hack
    version: "^0.5"  # You can specify any semver range

- name: Run cargo hack
  run: cargo hack --version
```

If no version is specified, the latest version will be installed. The
`--locked` flag is added by default to avoid breakages due to unexpected
dependencies updates.

### Inputs
- `crate` *(required)*: Name of the crate to install.
- `version`: Version to install (defaults to the latest version). Supports any
  semver range.
- `features`: Space or comma-separated list of crate features to enable.
- `locked`: Use the crate `Cargo.lock` if available (enabled by default).
  This adds `--locked` to the install command arguments.
- `args`: Additional arguments to pass to `cargo install`.
- `cache-key`: Additional string added to the cache key used to manually
  invalidate the cache.

### Outputs
- `version`: The version of the crate that has been installed.
- `cache-hit`: A boolean indicating whether the crate was restored from cache.

## Caching
Compiled binaries of installed crates are automatically cached. If a cached
version is present when the action is executed, it will be used. This allows
the installation of the crate to be almost instant in most cases.

Cached binaries will be automatically removed by GitHub if they have not been
accessed in the last 7 days. Read [Caching dependencies to speed up workflows]
to learn more about caching with GitHub Actions.

<details>
  <summary><strong>Cache key details</strong></summary>

  The `~/.cargo-install/<crate-name>` folder is cached with a cache key that
  follows the following pattern:

  ```
  cargo-install-<crate>-<version>-<hash>
  ```

  The hash is derived from the action job and runner os name and the
  installation arguments. The `cache-key` value is added to the hashed string
  if provided.
</details>

## Security
Crates are installed using `cargo install` and the latest version is retrieved
with the [crates.io] API. You can ask to install a specific version by not
using any semver range operator.

## Contributing
There is no particular contribution guidelines, feel free to open a new PR to
improve the code. If you want to introduce a new feature, please create an
issue before.

[changelog]: https://github.com/baptiste0928/cargo-install/releases/tag/v2.0.0
[crates.io]: https://crates.io
[`cargo-hack`]: https://crates.io/crates/cargo-hack
[Quickstart for GitHub Actions]: https://docs.github.com/en/actions/quickstart
[Caching dependencies to speed up workflows]: https://docs.github.com/en/actions/advanced-guides/caching-dependencies-to-speed-up-workflows
