# Struct, Implementation and Traits

There is no `class` and no inheritance. Rust has only `struct`, `impl` and `trait`.

## Struct

A `struct`is like an object's data attibute.

```rust
struct Rectangle {
	width: u32,
	height: u32,
}

let rect = Rectangle { width: 3, height: 2 };
rect.width; // => 3
```

[The Basics | Rest-and-Destructuring](https://cedeber.atlassian.net/wiki/spaces/RUSTO/pages/459128/The+Basics#Rest-and-Destructuring)

## Interface

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

The `::` syntax in the `::new` line indicates that new is an associated function of the `Rectangle` type. An associated
function is implemented on a type, rather than on a particular instance of a `Rectangle`. Some language call this a _
static method_.

> You will find a new function on many types, even in the standard library, because it's a common name for a function
> that makes a new value of some kind.

## Generics

`<T>`
