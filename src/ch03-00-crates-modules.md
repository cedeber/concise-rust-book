# Crates and Modules

Rust has a specific way of declaring things.

By default, Rust brings only a few types into the scope of every program in the prelude. If a type you want to use is not in the prelude, you have to bring that type into scope explicitly with a `use` statement.

## Hierarchy

### Crate

A crate may contains a lot of modules. A crate is a folder where, in its root, you have a `Cargo.toml` file.

> You can compare it to package.json in the JavaScript world, although we used to call it a node module. You import it with a single name, the node module name.

### Modules

- A module is a file.
- Or a folder with a mod.rs file inside.

> In the JavaScript world this is a module, or an ES module. This is when you import a file with a relative path instead of a name.

### Functions

Every module exports functions.

## Usage

```rust
use std::cmp::min(3, 8); // => 3
use std::cmp::\*
```

The JavaScript equivalent would be, considering that the folder `cmp` contains an `index.js` file.

```javascript
import { min } from "std/cmp";
import { * as cmp } from "std/cmp";

// or inside "std"
import { min } from "./cmp";
```

### Types are namespace too

```rust
let x = "rust".len(); // => 4
let x = str::len("rust"); // => 4
```

## Prelude

Rust inserts this at the beginning of every module.

```rust
use std::prelude::v1::\*;
```

So that, instead of `std::vec::Vec::new()`, you can use `Vec::new()`.
