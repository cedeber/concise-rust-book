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

# Option and Result

## The billion dollars mistake

Rust does not have `null` or `nil`. It is replaced by the `Option` enumeration. The variants of `Option` if `Some`
and `None`. The `None` variant represents no value while `Some` can hold one piece of data of any type.

Although some languages like _Kotlin_ or _Swift_ do enforce _strict null check_, as a code reader you never now if a
value can be `null` or not. Again, in Rust, this is self-explanatory and explicit.

```rust
// As defined in Rust
enum Option<T> {
	None,
	Some(T),
}
```

```rust
let val1: Option<i32> = None;
let val2: Option<i32> = Some(32);

println!("{:?}, {:?}", val1, val2);
```

For instance in JavaScript (or TypeScript without the `strictNullCheck`)

```javascript
class Foo {
  name = "foo";

  bar() {
    console.log(this.name, "bar");
  }
}

const fb = Math.random() < 0.5 ? new Foo() : null;

fb.bar(); // => TypeError: null is not an object
```

As `null` doesn't exist in Rust, you need the `Option` type which forces you to check the nullity `None`;

```rust
use rand::prelude::*;

struct Foo {
	name: String,
}

impl Foo {
	fn new() -> Self {
		Foo { name: "foo".to_string() }
	}

	fn bar(&self) {
		println!("{} bar", self.name);
	}
}

let fb: Option<Foo> = if rand::random() {
    Some(Foo::new())
} else {
    None
};

fb.bar()
// ^^^ method not found in `Option<Foo>`
```

## Error management

There is no `try` / `catch`. `Result` is, like `Option`, also an enumeration. For `Result`, the variants are `Ok`
and `Err`. The `Ok` variant indicates the operation was successful, and inside `Ok` is the successfully generated value.
The `Err` variant means the operation failed, and `Err` contains information about how or why the operation failed.

The purpose of these `Result` which is `Ok(_)` or `Err(_)`.

> A function always returns a unit `()`, if nothing is specified.

```rust
use std::error::Error;

fn main() -> Result<(), Box<dyn Error>> {
	Ok(())
}
```

> ⚠️ Talk about `unwrap` and `?`
> If you don’t call `except`, the program will compile, but you will get a warning
