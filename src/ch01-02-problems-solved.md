# Problems solved

## The billion dollars mistake

Rust does't have `null` or `nil`. It is replaced by the `Option` enum.
Althought some languages like _Kotlin_ or _Swift_ do enforce _strict null check_, as a code reader you never now if a value can be `null` or not. Again, in Rust, this is self-explanatory and explicit.

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

## Error management

There is no `try` / `catch`. A function returns a `Result` which is `Ok(_)` or `Err(_)`.

In addition a function always returns a _unit_ `()`, if nothing is specified.

```rust
# use std::error::Error;

fn main() -> Result<(), Box<dyn Error>> {
	Ok(())
}
```

## Inheritance

There is no `class` and no _inheritance_. Rust has only `struct` , `impl` and `trait`.

```rust
// An "interface" to implement
trait Geometry {
	fn area(&self) -> u32;
}

struct Rectangle {
	width: u32,
	height: u32,
}

// How to add functionality to a struct
impl Rectangle {
// Common Rust way to do a "constructor"
	fn new(width: u32, height: u32) -> Self {
		Self { width, height }
	}
}

// Implement a trait
impl Geometry for Rectangle {
	fn area(&self) -> u32 {
		self.width * self.height
	}
}

fn main() {
	// Init a struct
	let rect1 = Rectangle {
		width: 30,
		height: 50,
	};

	// Use the constructor instead
	let rect2 = Rectangle::new(20, 30);

	println!(
		"The area of the rectangle 1 is {} square pixels.",
		rect1.area()
	);

	println!(
		"The area of the rectangle 2 is {} square pixels.",
		rect2.area()
	);
}
```
