# Particularities

Rust has some particularities and also tries to fix other languages known issues.

## Ownership, Borrowing and Lifetime

Probably the most known particularities of Rust.
To sum it up very quickly: Every value is owned by one single variable, during a single lifetime.

This first example will fail as name has been passed to `_p` and is not owned by `name` anymore.

```rust
struct Person {
    name: String,
}

fn main() {
    let name = format!("Hello, {}!", "World");
    //  ---- move occurs because `name` has type `String`
    //       which does not implement the `Copy` trait

    let _p = Person { name };
    //                ---- value moved here

    println!("{}", name);
    //             ^^^^ value borrowed here after move
}
```

It can be borrowed, though. For that you need to use _references_ and _lifetime_ tags.

References passed to `Person<'a>` will now be alive as long as its usage, which is the _implicit lifetime_ of `main()`.

```rust
struct Person<'a> {
    name: &'a str,
}

fn main() {
    let name = format!("Hello, {}!", "World");
    let _p = Person { name: &name };

    println!("{}", name);
}
```

Or simply copy or clone the value. `Copy` is a shallow copy, while `Clone` is a deep clone.

```rust
struct Person {
    name: String,
}

fn main() {
    let name = format!("Hello, {}!", "World");
    let _p = Person { name: name.clone() };

    println!("{}", name);
}
```

## The billion dollars mistake

Rust does't have `null` or `nil`. It is replaced by the `Option` enum.
Although some languages like _Kotlin_ or _Swift_ do enforce _strict null check_, as a code reader you never now if a value can be `null` or not. Again, in Rust, this is self-explanatory and explicit.

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
// JavaScript
class Foo {
  name = "foo";

  bar() {
    console.log(this.name, "bar");
  }
}

const fb = Math.random() < 0.5 ? new Foo() : null;

fb.bar(); // => TypeError: null is not an object
```

As null doesn't exist in Rust, you need the `Option` type which forces you to check the nullity `None`;

```rust
# use rand::prelude::*;
#
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

## Unsafe

In order to write `unsafe` code, or, in other words, if you want to manage the memory yourself,
you can put your code in an `unsafe` block.

```rust
unsafe {
    // Your unsafe code
}
```

At this level of usage, I don't think you need this documentation anymore.

## Modules ?
