# Generic Types, Traits and Lifetimes

## Generics

Generics are abstract stand-ins for concrete types or other properties.

We can use generics to create definitions for items like function signatures or structs, which we can then use with many different concrete data types.

To define the generic `largest` function, place type name declaration inside angle brackets, `<>`, between the name of the function and the parameter list.

```rust
fn largest<T>(list: &[T]) -> T {}
```

We can also define structs to use a generic type parameter in one or more fields using the `<>` syntax.

```rust
struct Point<T> {
	x: T,
	y: T,
}
```

To define a `Point` struct where `x` and `y` are both generics but could have different types, we can use multiple generic type parameters.

```rust
struct Point<T, U> {
	x: T,
	y: U,
}
```

> You can use as many generic type parameters in a definition as you want, but using more than a few makes your code hard to read.

We can define enums to hold generic data types in their variants as we just see in `Option<T>` that returns `Some<T>` for instance.

We can also implement methods on structs and enums and use generic types in their definitions, too.

```rust
struct Point<T> {
	x: T,
	y: T,
}

impl<T> Point<T> {
	fn x(&self) -> &T {
		&self.x
	}
}
```

Note that we have to declare `T` just after `impl` so we can use it to specify that we are implementing methods on the type `Point<T>`. By declaring `T` as a generic type after `impl`, Rust can identify that the type in the angle brackets in `Point` is a generic type rather than a concrete type.

We could, for example, implement methods only on `Point<f32>` instances rather than on `Point<T>` instances with a generic type.

```rust
impl Point<f32> {
	fn distance_form_origin(&self) -> f32 {
		(self.x.powi(2) + self.y.powi(2)).sqrt()
	}
}
```

Generic type parameters in a struct definition aren't always the same as those you use in that struct`s method signatures.

```rust
struct Point<T, U> {
	x: T,
	y: U,
}

impl<T, U> Point<T, U> {
	fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> {
		Point {
			x: self.x
			y: other.y
		}
	}
}
```

> Rust implements generics by performing monomorphization of the code that is using generics at compile time. _Monomorphization_ is the process of turning generic code into specific code by filling in the concrete types that are used when compiled. With that, there is no runtime cost.

## Traits

Trait definitions are a way to group method signatures together to define a set of behaviours necessary to accomplish some purpose. They are some kind of _interfaces_.

```rust
trait Geometry {
	fn area(&self) -> u32;
}

struct Rectangle {
	width: u32,
	height: u32,
}

impl Rectangle {
	// Common Rust way to do a "constructor"
	fn new(width: u32, height: u32) -> Self {
		Self { width, height }
	}
}

impl Geometry for Rectangle {
	fn area(&self) -> u32 {
		self.width * self.height
	}
}

fn main() {
	let rect = Rectangle::new(20, 30);

	println!(
		"The area of the rectangle is {} square pixels.",
		rect.area()
	);
}
```

The difference is that after `impl`, we put the trait name that we want to implement, then use the `for`keyword, and then specify the name of the type we want to implement the trait for. Within the `impl` block, we put the method signatures that the trait definition has defined.

---

The `::` syntax in the `::new` line indicates that new is an associated function of the `Rectangle` type. An associated
function is implemented on a type, rather than on a particular instance of a `Rectangle`. Some language call this a _
static method_.

> You will find a new function on many types, even in the standard library, because it's a common name for a function
> that makes a new value of some kind.