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
		_ => println!("{}", n.value),
	}
}
```

## Rest and Destructuring

The rest operator `..` allows to fill the _holes_. Unlike the spread operator `...` in JavaScript, the rest
operator `..` comes at the end. In other words you are _not overwriting the values_, but you are **filling the missing
ones**. It is called the _struct update syntax_.

> The rest must be the last and not be followed by a comma.

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
let Vec2 { x,..} = v2;

println! {"{:?}, {:?}, {:?}", v2, b, x} // => Vec2 { x: 1.0, y: 2.0 }, 7, 1.0
```
