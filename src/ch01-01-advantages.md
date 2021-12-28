# Advantages

## Modern

- Typed language
- General purpose
- Explicit

## Safe and Memory efficient

- _Ownership_ and _Borrowing_
- No Garbage collector
- Fast (like C/C++)

## Type inference

```rust
let str = "I am not really a String ;-)"; // -> &str
let val = 40_000 // -> i32
let val = 40. // -> f64
```

## Immutable by default

```rust
let foo = 32;
foo = 40; // Nope
```

Let's make it mutable.

```rust
let mut foo = 32;
foo = 40;
```

## Variable names are reusable

```rust
let x = 32;
let x = 32 + 8; // still immutable!

println!("{}", x); // Only the last x will be available
```

- A complete ecosystem (`rustup`, `cargo`, `crates`, `rustfmt`, `clippy`)
- Good IDE support
- Documentation in source code
- Examples folder managed with `cargo`
- Tests in source code: `cargo test`
- `cargo` can be expanded: `cargo-edit`, `cargo-watch`, ...
- Compiler giving hints
