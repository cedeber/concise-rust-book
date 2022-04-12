# Useful Crates

This list is obviously very **opiniated** and **not complete**. It's a list a useful crates I discovered while using Rust. Some better-known crates are not displayed below, because I prefer the ones listed here.

## General purpose

| Crate Name                                          | Description                                                                                             |
| --------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| [itertools](https://crates.io/crates/itertools)     | Extra iterator adaptors, iterator methods, free functions, and macros.                                  |
| [tokio](https://crates.io/crates/tokio)             | An event-driven, non-blocking I/O platform for writing asynchronous I/O backed applications.            |
| [serde](https://crates.io/crates/serde)             | A generic serialization/deserialization framework.                                                      |
| [rayon](https://crates.io/crates/rayon)             | Simple work-stealing parallelism for Rust.                                                              |
| [futures](https://crates.io/crates/futures)         | Futures and streams featuring zero allocations, composability, and iterator-like interfaces.            |
| [lazy_static](https://crates.io/crates/lazy_static) | A macro for declaring lazily evaluated statics in Rust.                                                 |
| [ron](https://crates.io/cratess/ron)                | Rusty Object Notation                                                                                   |
| [fluent](https://crates.io/crates/fluent)           | A localization system designed to unleash the entire expressive power of natural language translations. |

## Data Types

| Crate Name                              | Description                                                                                                |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| [regex](https://crates.io/crates/regex) | An implementation of regular expressions for Rust.                                                         |
| [num](https://crates.io/crates/num)     | A collection of numeric types and traits (bigint, complex, rational, range iterators, generic integers...) |
| [uuid](https://crates.io/crates/uuid)   | A library to generate and parse UUIDs.                                                                     |

## CLI

| Crate Name                                      | Description                                            |
| ----------------------------------------------- | ------------------------------------------------------ |
| [log](https://github.com/rust-lang/log)         | A Rust library providing a lightweight logging facade. |
| [clap](https://github.com/clap-rs/clap)         | Command Line Argument Parser for Rust.                 |
| [console](https://github.com/mitsuhiko/console) | A Rust console and terminal abstraction.               |

## Database

| Crate Name                            | Description                                                 |
| ------------------------------------- | ----------------------------------------------------------- |
| [sqlx](https://crates.io/crates/sqlx) | An async SQL crate. Supports PostgreSQL, MySQL, and SQLite. |
| [slab](https://crates.io/crates/slab) | Pre-allocated storage for a uniform data type.              |

## Web

| Crate Name                                                | Description                                                                           |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| [hyper](https://crates.io/crates/hyper)                   | A fast and correct HTTP library.                                                      |
| [axum](https://crates.io/crates/axum)                     | Web framework that focuses on ergonomics and modularity.                              |
| [async-graphql](https://crates.io/crates/async-graphql)   | A GraphQL server library implemented in Rust.                                         |
| [lettre](https://crates.io/crates/lettre)                 | Email client                                                                          |
| [reqwest](https://crates.io/crates/reqwest)               | Higher level HTTP client library.                                                     |
| [tungstenite](https://crates.io/crates/async-tungstenite) | Async binding for Tungstenite, the Lightweight stream-based WebSocket implementation. |
