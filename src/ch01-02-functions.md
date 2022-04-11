# Functions

## Functions

Every function returns, if not specified, a **unit** type, written `()`. No need to specify it or return it, it is implicit.

```rust
fn main() -> () {
    // empty
}
```

### Semi colon

A semi-colon is mandatory at the end of each expressions. If it is _omitted_, it means a `return`.

> Take care of this one!

```rust
fn add_2(a: i32) -> i32 {
    a + 2
}

println!("{}", add_2(5)); // => 7
```

### Closure

```rust
let x = vec![1, 2, 3, 4]
    .iter()
    .map(|x| x + 3);

let closure_annotated = |i: i32| -> i32 { i + 1 };
let closure_inferred = |i | i + 1 ;
```

## Blocks and Scope

A pair of brackets declares a block, which has its own scope.

```rust
fn main() {
    let x = "out";
    {
        // this is a different `x`
        let x = "in";
        println!("{x}"); // => in
    }
    println!("{x}"); // => out
}
```

### Blocks are also expressions

They can evaluate to a value and can contain multiple statements.

```rust
let x = {
    let y = 1;
    let z = 2;
    y + z
};

println!("{x}"); // => 3
```

#### if conditionals

```rust
fn eight() -> i32 {
    if true {
        8
    } else {
        4
    }
}

println!("{}", eight()); // => 8
```

#### match pattern

```rust
use rand::prelude::\*;

fn dice(cheat: bool) -> i32 {
    match cheat {
        true => 6,
        false => thread_rng().gen_range(0..=6),
    }
}

println!("{}", dice(true)); // => 6
```

## Rest and Destructuring

The rest operator `..` allows to fill the _holes_. Unlike the spread operator `...` in JavaScript, the rest operator `..` comes at the end. In other words you are _not overwriting the values_, but you are **filling the missing ones**.

The rest must be the last and not be followed by a comma.

```rust
#[derive(Debug)]
struct Vec2 {
    x: f32,
    y: f32
}

// Rest
let v1 = Vec2 { x: 1.0, y: 3.0 };
let v2 = Vec2 { y: 2.0, ..v1 };

// Destructuring
let (a, b) = (3, 7);

// Destructuring with Rest
let Vec2 { x, .. } = v2;

println!{"{:?}, {:?}, {:?}", v2, b, x} // => Vec2 { x: 1.0, y: 2.0 }, 7, 1.0
```
