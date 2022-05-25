# Rust Setup

## Crate setup

You must define properly the `crate-type`. The `cdylib` provides interoperability with C code.<br>
In addition, `rlib` indicates that a Rust library will be produced.

> Please refer to [Chapter 3](ch03-00-crates-modules.md) to know more about packages, crates and modules.

```toml
# Cargo.toml

[lib]
crate-type = ["cdylib", "rlib"]
```

### Useful Crates

- [`console_error_panic_hook`](https://crates.io/crates/console_error_panic_hook): This crate provides better debugging of panics by logging them with `console.error`. This is great for development, but requires all the `std::fmt` and `std::panicking` infrastructure, so isn't great for code size when deploying.
- [`wee_alloc`](https://crates.io/crates/wee_alloc): This is a tiny allocator for wasm that is only `~1K` in code size compared to the default allocator's `~10K`. It is slower than the default allocator, however.
- [`wasm-bindgen`](https://crates.io/crates/wasm-bindgen), [`wasm-bindgen-futures`](https://crates.io/crates/wasm-bindgen-futures): Easy support for interacting between JS (Promises) and Rust (Futures).
- [`log`](https://crates.io/crates/log), [`console_log`](https://crates.io/crates/console_log): A logging facility that routes Rust log messages to the browser's console.
- [`web-sys`](https://crates.io/crates/web-sys), [`js-sys`](https://crates.io/crates/js-sys): Bindings for WEB APIs and all JS global objects and functions.
- [`gloo`](https://crates.io/crates/gloo): A modular toolkit for Rust and WebAssembly. Wrappers around `web-sys` and `js-sys`.

### Cargo.toml

```toml
[package]
name = "wasm_crate"
version = "0.1.0"
edition = "2021"
publish = false

[lib]
crate-type = ["cdylib", "rlib"]

[features]
default = ["console_error_panic_hook", "wee_alloc", "console_log"]

[dependencies]
console_error_panic_hook = { version = "0", optional = true }
wee_alloc = { version = "0", optional = true }
wasm-bindgen = { version = "0", features = ["serde-serialize"] }
wasm-bindgen-futures = "0"
js-sys = "0"
web-sys = "0"
log = "0"
console_log = { version = "0", features = ["color"], optional = true }

[dev-dependencies]
wasm-bindgen-test = "0"
```

### src/lib.rs

```rust,noplayground
use log::{info, Level};
use wasm_bindgen::prelude::*;

#[cfg(feature = "wee_alloc")]
#[global_allocator]
static ALLOC: wee_alloc::WeeAlloc = wee_alloc::WeeAlloc::INIT;

#[wasm_bindgen(start)]
pub fn main_wasm() -> Result<(), JsValue> {
	#[cfg(feature = "console_error_panic_hook")]
	console_error_panic_hook::set_once();

	#[cfg(feature = "console_log")]
	console_log::init_with_level(Level::Trace).expect("error initializing log");

	Ok(())
}
```
