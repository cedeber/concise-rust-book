# Introduction

A quick introduction with concepts you may have seen on other programming languages.
At least they are very easy concepts to understand as for now.

## Type inference

You don't need to define the type on assignment.

```rust
let str = "I am not really a String ;-)"; // -> &str
let a = 40_000; // -> i32
let b = 40.; // -> f64
let v: [_; 5] = [3; 5];
//      ^ infers type for usage
```

## Underscores

There is a lot to say about the usage of underscores. Here are just few examples with variables.

### Unused variables

While you are coding, you can say the compiler to not complain about a variable not being used.

```rust
let _unused_variable = 42;
```

### Throw away a value

During _destructuring_, if you don't to deal with all values you can omit some with an underscore.
In use with the rest operator `..` it's very easy to just export what you need.

```rust
let (_, b) = (3, 7); // The value 3 will not be assigned to a variable
```

## Immutable by default

Mutability in Rust is explicit.

```rust
# struct Number { value: i32 }
#
let foo = 32;
foo = 40; // Nope

let n = Number {
    value: 3
};
n.value = 5; // Nope
n = Number { value: 5 } // Nope
```

Let's make it **mutable**.

```rust
# struct Number { value: i32 }
#
let mut foo = 32;
foo = 40; // Yep

let mut n = Number {
    value: 3
};
n.value = 5; // Yep
n = Number { value: 5 } // Yep
```

### Variable names are reusable

```rust
let x = 32;
let x = 32 + 8; // still immutable!

println!("{}", x); // => 40. Only the last x will be available
```

### Mutable reference

> Note: Of course this in not a good practice and a function should avoid to have side effect like this.
> But sometimes this is needed, like when you want to update a graphical context for example.

```rust
// This function will have side effects
fn change(a: &mut i32) {
    *a = 5
}

let mut a: i32 = 3;
change(&mut a); // Explicit mutability. You have been warned.

println!("{}", a); // => 5
```

## Functions

Every function returns, if not specified, a `unit` type, written `()`.
No need to specify it or return it, it is implicit.

```rust
fn main() -> () {
    // empty
}
```

### Semi-colon

A semi-colon is mandatory at the end of each expressions.
If it is _omitted_, it means a `return`.

> Note: Take care of this one!

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
let closure_inferred  = |i     |          i + 1  ;
```

## Blocks and Scope

```rust
fn main() {
    let x = "out";
    {
        // this is a different `x`
        let x = "in";
        println!("{}", x); // => in
    }
    println!("{}", x); // => out
}
```

### Blocks are also expressions.

```rust
let x = {
    let y = 1;
    let z = 2;
    y + z
};

println!("{}", x); // => 3
```

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

```rust
# use rand::prelude::*;
#
fn dice(cheat: bool) -> i32 {
    match cheat {
        true => 6,
        false => thread_rng().gen_range(0..=6),
    }
}

println!("{}", dice(true)); // => 6
```

## Rest & Destructuring

```rust
# #[derive(Debug)]
# struct Vec2 {
#     x: f32,
#     y: f32
# }
#
// Rest
let v1 = Vec2 { x: 1.0, y: 3.0 };
let v2 = Vec2 { y: 2.0, ..v1};

// Destructuring
let (a, b) = (3, 7);

// Destructuring with Rest
let Vec2 { x, .. } = v2;

println!{"{:?}, {:?}, {:?}", v2, b, x} // => Vec2 { x: 1.0, y: 2.0 }, 7, 1.0
```

## Patterns

### `let` pattern

```rust
let a = Some(40);
let b = None;

print_number(a); // => 40
print_number(b); // => No output

fn print_number(n: Option<i32>) {
    if let Some(value) = n {
        println!("{}", value)
    }
}
```

With destructuring and test in the same time:

```rust
struct Number {
    odd: bool,
    value: i32,
}

let one = Number { odd: true, value: 1 };
let two = Number { odd: false, value: 2 };

print_number(one); // => Odd number: 1
print_number(two); // => Even number: 2

fn print_number(n: Number) {
    if let Number { odd: true, value } = n {
        println!("Odd number: {}", value);
    } else if let Number { odd: false, value } = n {
        println!("Even number: {}", value);
    }
}
```

### `match` pattern

```rust
# struct Number {
#     odd: bool,
#     value: i32,
# }
#
# let one = Number { odd: true, value: 1 };
# let two = Number { odd: false, value: 2 };
#
# print_number(one); // => Odd number: 1
# print_number(two); // => Even number: 2
#
// Same as with the if pattern
fn print_number(n: Number) {
    match n {
        Number { odd: true, value } => println!("Odd number: {}", value),
        Number { odd: false, value } => println!("Even number: {}", value),
    }
}
```

A `match` has to be exhaustive. At least one arm needs to match.
`_` can be used as a "catch-all" pattern.

```rust
fn print_number(n: Number) {
    match n.value {
        1 => println!("One"),
        2 => println!("Two"),
        _ => println!("{}", n.value),
    }
}
```
