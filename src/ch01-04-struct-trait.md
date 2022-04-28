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

### Rest and Destructuring

The rest operator `..` allows to fill the _holes_. It is called the _struct update syntax_.

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

## Enums

Rust's enums are most similar to _algebraic data types_ in functional languages.
Note that the variants of the enum are namespaced (`::`) under its identifier.

We can put data directly into each enum variant. Each variant can have different types and amount of associated data.

```rust
enum IpAddr {
	V4(u8, u8, u8, u8),
	V6(String),
}

let home = IpAddr::V4(127, 0, 0, 1);
let loopback = IpAddr::V6(String::from("::1"));
```

You can put any kind of data inside an enum variant: strings, numeric types, or struct, for example. You can even include another enum.

### The billion dollars mistake

The problem with `null` values is that if you try to use a `null` value as a not-null value, you will get an error of some kind.
As such, Rust does not have nulls, but it does have an enum that can encode the concept of a value being present or absent.

It is replaced by the `Option<T>` enumeration. The variants of `Option` are `Some`
and `None`. The `None` variant represents no value while `Some` can hold one piece of data of any type.

```rust
enum Option<T> {
	Some(T),
	None,
}
```

If we use `None` rather that `Some`, we need to tell Rust what type of `Option<T>` we have.

```rust
let val1: Option<i32> = None;
let val2: Option<_> = Some(32);

println!("{:?}, {:?}", val1, val2);
```

Because `Option<T>` and `T` (where `T` can be any type) are different types, the compiler won't let us use an `Option<T>` value as if it were definitely a valid value.

```rust
let x: i8 = 5;
let y: Option<i8> = Some(5);

let sum = x + y;
//          ^ no implementation for `i8 + Option<i8>`
```

> Everywhere that a value has a type that is not an `Option<T>`, you _can_ safely assume that the value is not null.

### Error management

Rust doesn't have exceptions. Instead, it has the type the type `Result<T, E>` for recoverable errors and the `panic!` macro that stops execution when the program encounters an unrecoverable error.

`Result` is, like `Option`, also an enumeration. For `Result<T, E>`, the variants are `Ok<T>` and `Err<E>`. The `Ok` variant indicates the operation was successful, and inside `Ok` is the successfully generated value.
The `Err` variant means the operation failed, and `Err` contains information about how or why the operation failed.

```rust
use std::fs::File;

let f = File::open("hello.txt");
let f = match f {
	Ok(file) => file,
	Err(error) => {
		panic!("Problem opening the file: {:?}", error)
	}
}
```

#### Shortcuts for Panic on Error: unwrap and expect

Using `match` can be a bit verbose. The `Result<T, E>` type has many helper methods.

One of those method is called `unwrap`. If the `Result` value is the `Ok` variant, `unwrap` will return the value inside the `Ok`. If the `Result` is the `Err` variant, `unwrap` will call the `panic!` macro.

```rust
let f = File::open("hello.txt").unwrap();
```

Another method, `expect`, which is similar to `unwrap`, lets us also choose the `panic!` error message.

```rust
let f = File::open("hello.txt").expect("Failed to open hello.txt");
```

> It would be appropriate to call `unwrap` when you have some other logic that ensures the `Result` will have an `Ok`value, but the logic isn't something the compiler understands.

#### Propagating Errors

This pattern of propagating errors is so common in Rust that Rust provides the question mark operator `?` to make this easier. Error values that have the `?` operator called on them go through the `from` function, defined in the `From` trait in the standard library, which is used to convert errors from one type into another.

the `?` operator eliminates a lot of boilerplate and makes this function's implementation simpler. We could event shorten the code further by chaining method calls immediately after the `?`.

```rust
fn read_username_from_file() -> Result<String, io::error> {
	let mut s = String::new();
	File::open("hello.txt")?.read_to_string(&mut s)?;
	Ok(s);
}
```

> The `?` operator can only be used in functions that have a return type of `Result`.

```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
	let f = File::open("hello.txt")?;

	Ok(());
}
```

The `Box<dyn Error>` type is called a trait object. For now, you can read `Box<dyn Error>` to mean "any kind of error".

## Traits

Traits are like interfaces.

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
