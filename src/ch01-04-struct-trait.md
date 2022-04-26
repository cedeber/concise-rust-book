# Struct, Implementation and Traits

There is no `class` and no inheritance. Rust has only `struct`, `impl` and `trait`.

## Struct

A `struct` is like an object's data attribute.

> It's possible for structs to store references to data owned by something else, but to do so requires the use of _lifetimes_.

```rust
struct Rectangle {
	width: u32,
	height: u32,
}

let rect = Rectangle { width: 3, height: 2 };
rect.width; // => 3
```

### Tuple Structs

Tuple structs are useful when you want to give the whole tuple a name and make the tuple be a different type from other tuples, and naming each field as in a regular struct would be verbose or redundant.

```rust
struct Color(i32, i32, i32);
let black = Color(0, 0, 0);
```

### Unit-Like Structs

You can also define structs that don't have any fields! These are called _unit-like structs_ because they behave similarly to `()`, the unit type. Unit-like structs can be useful in situations in which you need to implement a trait on some type but don't have any data that you want to store in the type itself.

## Method Syntax: Implementation

```rust
struct Rectangle {
	width: u32,
	height: u32,
}

impl Rectangle {
	fn area(&self) -> u32 {
		self.width * self.height
	}
}

let rect = Rectangle { width: 30, height: 50 };
println!("The are is {} square pixels.", rect.area());
```

Methods are similar to functions. To define the function within the context of `Rectangle`, we start an `impl` block. Then we move the `area` function within the `impl` curly brackets.

We can use the _method syntax_ to call the area method on our `Rectangle` instance.

In the signature for `area` we use `&self` instead of `rectangle: &Rectangle` because Rust knows the type of `self` is `Rectangle` due to this method's being inside the `impl Rectangle` context. Methods can take multiple parameters that we add to the signature after the `self` parameter, and those parameters work just like parameters in functions.

> Each struct is allowed to have multiple `impl` blocks.

### Associated functions

Another useful feature of `impl` blocks is that we are allowed to define functions within `impl` blocks that _do not_ take `self` as a parameter. These are called _associated functions_ because they are associated with the struct.

> They're still functions, not methods, because they don't have an instance of the struct to work with, like `String::from`.

Associated functions are often used for constructors that will return a new instance of the struct. A common usage in the standard library and in the community is to define a `new` function. You can use the _field init shorthand_ syntax to initialize a struct with variables.

```rust
impl Rectangle {
	fn new(width: u32, height: u32) -> Self {
		Rectangle { width, height };
	}
}
```

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
