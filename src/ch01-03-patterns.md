# Patterns

## let pattern

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
0print_number(two); // => Even number: 2

fn print_number(n: Number) {
    if let Number { odd: true, value } = n {
        println!("Odd number: {}", value);
    } else if let Number { odd: false, value } = n {
        println!("Even number: {}", value);
    }
}
```

## match pattern

```rust
struct Number {
    odd: bool,
    value: i32,
}

let one = Number { odd: true, value: 1 };
let two = Number { odd: false, value: 2 };

print_number(one); // => Odd number: 1
print_number(two); // => Even number: 2

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
        * => println!("{}", n.value),
    }
}
```
