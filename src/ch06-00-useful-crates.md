# Useful Crates

This list is obviously very **opiniated** and **not complete**. It's a list a useful crates I discovered while using Rust. Some better-known crates are not displayed below, because I prefer the ones listed here.

## General purpose

| Crate Name                                          | Description                                                                                  |
| --------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| [futures](https://crates.io/crates/futures)         | Futures and streams featuring zero allocations, composability, and iterator-like interfaces. |
| [itertools](https://crates.io/crates/itertools)     | Extra iterator adaptors, iterator methods, free functions, and macros.                       |
| [lazy_static](https://crates.io/crates/lazy_static) | A macro for declaring lazily evaluated statics in Rust.                                      |
| [rayon](https://crates.io/crates/rayon)             | Simple work-stealing parallelism for Rust.                                                   |
| [serde](https://crates.io/crates/serde)             | A generic serialization/deserialization framework.                                           |
| [serde_json](https://crates.io/crates/serde_json)   | A JSON serialization file format.                                                            |
| [tokio](https://crates.io/crates/tokio)             | An event-driven, non-blocking I/O platform for writing asynchronous I/O backed applications. |

## Math, Physics, Geo, Bio

| Crate Name                                    | Description                                                                                                                                      |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| [bio](https://crates.io/crates/bio)           | A bioinformatics library for Rust.                                                                                                               |
| [geo](https://crates.io/crates/geo)           | Geospatial primitives and algorithms.                                                                                                            |
| [nalgebra](https://crates.io/crates/nalgebra) | General-purpose linear algebra library with transformations and statically-sized or dynamically-sized matrices. [Dimforge](https://dimforge.com) |
| [num](https://crates.io/crates/num)           | A collection of numeric types and traits (bigint, complex, rational, range iterators, generic integers...)                                       |
| [parry2d](https://crates.io/crates/parry2d)   | 2 dimensional collision detection library in Rust. [Dimforge](https://dimforge.com)                                                              |
| [rand](https://crates.io/crates/rand)         | Random number generators and other randomness functionality.                                                                                     |

## String, Encoding, Time and Crypto

| Crate Name                                                            | Description                                                                                             |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| [chrono](https://crates.io/crates/chrono)                             | Date and time library for Rust.                                                                         |
| [crypto](https://crates.io/crates/crypto)                             | Resources for building cryptosystems in Rust using the RustCrypto project's ecosystem.                  |
| [data-encoding](https://crates.io/crates/data-encoding)               | Data-encoding functions like base64, base32, and hex.                                                   |
| [fluent](https://crates.io/crates/fluent)                             | A localization system designed to unleash the entire expressive power of natural language translations. |
| [regex](https://crates.io/crates/regex)                               | An implementation of regular expressions for Rust.                                                      |
| [unicode-segmentation](https://crates.io/crates/unicode-segmentation) | Grapheme Cluster, Word and Sentence boundaries according to Unicode Standard Annex #29 rules.           |
| [uuid](https://crates.io/crates/uuid)                                 | A library to generate and parse UUIDs.                                                                  |

## Files

| Crate Name                                      | Description                                                            |
| ----------------------------------------------- | ---------------------------------------------------------------------- |
| [dotenvy](https://crates.io/crates/dotenvy)     | A `dotenv` implementation for Rust.                                    |
| [roxmltree](https://crates.io/crates/roxmltree) | Represent an XML as a read-only tree.                                  |
| [ron](https://crates.io/cratess/ron)            | Rusty Object Notation                                                  |
| [toml](https://crates.io/crates/toml)           | A native Rust encoder and decoder of TOML-formatted files and streams. |

## CLI

| Crate Name                                      | Description                                            |
| ----------------------------------------------- | ------------------------------------------------------ |
| [clap](https://github.com/clap-rs/clap)         | Command Line Argument Parser for Rust.                 |
| [console](https://github.com/mitsuhiko/console) | A Rust console and terminal abstraction.               |
| [log](https://github.com/rust-lang/log)         | A Rust library providing a lightweight logging facade. |

## Database

| Crate Name                            | Description                                                 |
| ------------------------------------- | ----------------------------------------------------------- |
| [slab](https://crates.io/crates/slab) | Pre-allocated storage for a uniform data type.              |
| [sqlx](https://crates.io/crates/sqlx) | An async SQL crate. Supports PostgreSQL, MySQL, and SQLite. |

## Graphics and UI

| Crate Name                                    | Description                                                                                             |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| [image](https://crates.io/crates/image)       | Imaging library written in Rust. Provides basic filters and decoders for the most common image formats. |
| [piet](https://crates.io/crates/piet)         | An abstraction for 2D graphics.                                                                         |
| [piet-svg](https://crates.io/crates/piet-svg) | SVG backend for piet 2D graphics abstraction.                                                           |

## Web

| Crate Name                                                | Description                                                                                    |
| --------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| [async-graphql](https://crates.io/crates/async-graphql)   | A GraphQL server library implemented in Rust.                                                  |
| [axum](https://crates.io/crates/axum)                     | Web framework that focuses on ergonomics and modularity.                                       |
| [hyper](https://crates.io/crates/hyper)                   | A fast and correct HTTP library.                                                               |
| [jsonwebtoken](https://crates.io/crates/jsonwebtoken)     | Create and decode JWTs in a strongly typed way.                                                |
| [lettre](https://crates.io/crates/lettre)                 | Email client                                                                                   |
| [reqwest](https://crates.io/crates/reqwest)               | Higher level HTTP client library.                                                              |
| [tower](https://crates.io/crates/tower)                   | Tower is a library of modular and reusable components for building robust clients and servers. |
| [tower-http](https://crates.io/crates/tower-http)         | Tower middleware and utilities for HTTP clients and servers.                                   |
| [tungstenite](https://crates.io/crates/async-tungstenite) | Async binding for Tungstenite, the Lightweight stream-based WebSocket implementation.          |

## WebAssembly

| Crate Name                                                                    | Description                                                                    |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| [console_error_panic_hook](https://crates.io/crates/console_error_panic_hook) | A panic hook for `wasm32-unknown-unknown` that logs panics to `console.error`. |
| [js-sys](https://crates.io/crates/js-sys)                                     | Bindings for all JS global objects and functions in all JS environments.       |
| [piet-web](https://crates.io/crates/piet-web)                                 | Web canvas backend for piet 2D graphics abstraction.                           |
| [wasm-bindgen](https://crates.io/crates/wasm-bindgen)                         | Easy support for interacting between JS and Rust.                              |
| [wasm-bindgen-futures](https://crates.io/crates/wasm-bindgen-futures)         | Bridging the gap between Rust Futures and JavaScript Promises.                 |
| [wasm-bindgen-rayon](https://crates.io/crates/wasm-bindgen-rayon)             | Adapter for using Rayon-based concurrency on the Web.                          |
| [web-sys](https://crates.io/crates/web-sys)                                   | Bindings for all Web APIs.                                                     |
| [wee_alloc](https://crates.io/crates/wee_alloc)                               | The Wasm-Enabled, Elfin Allocator.                                             |
