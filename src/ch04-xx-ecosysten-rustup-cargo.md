# Rustup

[`rustup`](https://rustup.rs) installs The Rust Programming Language from the official release channels, enabling you to easily switch between stable, beta, and nightly compilers and keep them updated. It makes cross-compiling simpler with binary builds of the standard library for common platforms. And it runs on all platforms Rust supports.

[`rustup`](https://rustup.rs) is a toolchain multiplexer. It installs and manages many Rust toolchains and presents them all through a single set of tools installed to `~/.cargo/bin`. The [`rustc`](https://doc.rust-lang.org/rustc/) and [`cargo`](https://doc.rust-lang.org/cargo/) executables installed in `~/.cargo/bin` are proxies that delegate to the real toolchain. rustup then provides mechanisms to easily change the active toolchain by reconfiguring the behavior of the proxies.

This is similar to Ruby's `rbenv`, Python's `pyenv`, or Node's `nvm`.

See more on the [`rustup` documentation](https://rust-lang.github.io/rustup/index.html).

# Cargo

Cargo is the Rust package manager. Cargo downloads your Rust package's dependencies, compiles your packages, makes distributable packages, and uploads them to [crates.io](https://crates.io), the Rust community’s package registry.

See more on the [`cargo` documentation](https://doc.rust-lang.org/cargo/index.html)

You can use `cargo new` or `cargo init` to start a new project.

## Running and Building

`cargo run` is probably the first command you want to use in order to see the output of your software. It runs a binary or example of the local package. All the arguments following the two dashes (`--`) are passed to the binary to run.

`cargo build` compiles local packages and all of their dependencies.

## Checking and Linting

`cargo check` checks a local package and all of its dependencies for errors. This will essentially compile the packages without performing the final step of code generation, which is faster.

`cargo fix` automatically takes rustc's suggestions from diagnostics like warnings and apply them to your source code. This is intended to help automate tasks that rustc itself already knows how to tell you to fix!

### Clippy

Clippy is a linter which is more features complete than what the compiler can provide with `cargo check` or `cargo fix`.

It is usually installed thanks to `rustup` and you can lint your code with `cargo clippy`. In order to apply the fix, run `cargo clippy --fix`

Usually Clippy is well integrated into the IDE but needs to be activated. As you can imagine it needs more power and time to run properly, but beleave me, it's an incredible tool.

See more on the [Clippy GitHub repository](https://github.com/rust-lang/rust-clippy)

## Extend Cargo

Cargo can be extanded via plugin. The most useful one is probably `cargo-edit` which allow you to add, remove, and upgrade dependencies by modifying your `Cargo.toml` file from the command line. You can simply install it by running `cargo install cargo-edit`.

See more on the [cargo-edit crate page](https://crates.io/crates/cargo-edit).

## Other

Cargo can do more, it is detailed in the next chapters.
