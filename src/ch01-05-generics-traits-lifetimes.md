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

Trait definitions are a way to group method signatures together to define a set of behaviors necessary to accomplish some purpose. They are some kind of _interfaces_.

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

The difference is that after `impl`, we put the trait name that we want to implement, then use the `for` keyword, and then specify the name of the type we want to implement the trait for. Within the `impl` block, we put the method signatures that the trait definition has defined.

One restriction to note with trait implementations is that we can implement a trait on a type only if at least one of the trait or the type is local to our crate. But we can’t implement external traits on external types. This restriction is part of a property of programs called _coherence_, and more specifically the _orphan rule_, so named because the parent type is not present. This rule ensures that other people’s code can’t break your code and vice versa. Without the rule, two crates could implement the same trait for the same type, and Rust wouldn’t know which implementation to use.

### Default Implementations

Sometimes it’s useful to have default behavior for some or all of the methods in a trait instead of requiring implementations for all methods on every type.

```rust
pub trait Summary {
    fn summarize(&self) -> String {
        String::from("(Read more...)")
    }
}
```

> To use a default implementation we specify an empty `impl` block. Default implementations can call other methods in the same trait, even if those other methods don’t have a default implementation.

### Traits as Parameters

We can define a function that calls the method on its parameter, which is of some type that implements the trait. To do this we can use the `impl Trait` syntax.

```rust
pub fn notify(item: &impl Summary) {
    println!("Breaking news! {}", item.summarize());
}
```

The `impl Trait` syntax works for straightforward cases but is actually syntax sugar for a longer form, which is called a _trait bound_.

```rust
pub fn notify<T: Summary>(item: &T) {
    println!("Breaking news! {}", item.summarize());
}
```

We can also specify more than one trait bound.

```rust
pub fn notify(item: &(impl Summary + Display)) {}
pub fn notify<T: Summary + Display>(item: &T) {}
```

Rust has alternate syntax for specifying trait bounds inside a `where` clause after the function signature.

```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{}
```

### Returning Types that Implement Traits

We can also use the 'impl Trait' syntax in the return position to return a value of some type that implements a trait.

```rust
fn returns_summarizable() -> impl Summary {
	Tweet {
		// --snip--
	}
}
```

The ability to return a type that is only specified by the trait it implements is especially useful in the context of closures and iterators. However you can only use the `impl Trait` if you are returning a single type.

```rust
fn returns_summarizable(switch: bool) -> impl Summary {
	if switch {
		NewsArticle {
			// --snip--
		}
	} else {
		Tweet {
			// --snip--
		}
	}
}
```

> Returning either a NewsArticle or a Tweet isn't allowed due to restrictions around how the `impl Trait` is implemented in the compiler.

### Derivable Traits

> TODO: Do the macro chapter first

### Using Trait Bounds to Conditionally Implement Methods

By using a trait bound with an `impl` block that uses generic type parameters, we can implement methods conditionally for types that implement the specified traits.

```rust
struct Pair<T> {
	x: T,
	y: T,
}

impl<T: Display + PartialOrd> Pair<T> {
	// --snip--
}
```

We can also conditionally implement a trait for any type that implements another trait. Implementations of a trait on any type that satisfies the trait bounds are called _blanket implementations_ and are extensively used in the Rust standard library.

```rust
impl<T: Display> ToString for T {
    // --snip--
}
```

> In dynamically typed languages, we would get an error at runtime if we called a method on a type which didn’t define the method. But Rust moves these errors to compile time so we’re forced to fix the problems before our code is even able to run. Additionally, we don’t have to write code that checks for behavior at runtime because we’ve already checked at compile time.

## Lifetimes

Most of the time, lifetimes are implicit and inferred, just like most of the time, types are inferred. We must annotate types when multiple types are possible. In a similar way, we must annotate lifetimes when the lifetimes of references could be related in a few different ways

The main aim of lifetimes is to prevent dangling references, which cause a program to reference data other than the data it is intended to reference.

### The Borrow Checker

The Rust compiler has a borrow checker that compares scopes to determine whether all borrows are valid.

```rust
fn main() {
	let r;                // ---------+-- 'a
						  //          |
	{                     //          |
		let x = 5;        // -+-- 'b  |
		r = &x;           //  |       |
	}                     // -+       |
						  //          |
	println!("r: {}", r); //          |
}                         // ---------+
```

```rust
fn longest(x: &str, y: &str) -> &str {
//            ----     ----     ^ expected named lifetime parameter
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

//   = help: this function's return type contains a borrowed value,
//           but the signature does not say whether it is borrowed from `x` or `y`
// help: consider introducing a named lifetime parameter
//   |
//   | fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
//   |           ++++     ++          ++          ++
```

### Lifetime annotation

Lifetime annotations have a slightly unusual syntax: the names of lifetime parameters must start with an apostrophe (`'`) and are usually all lowercase and very short, like generic types. Most people use the name `'a`. We place lifetime parameter annotations after the `&` of a reference, using a space to separate the annotation from the reference’s type.

```rust
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

As with generic type parameters, we need to declare generic lifetime parameters inside angle brackets between the function name and the parameter list. The constraint we want to express in this signature is that the lifetimes of both of the parameters and the lifetime of the returned reference are related such that the returned reference will be valid as long as both the parameters are.

```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
	// --snip--
}
```

Remember, when we specify the lifetime parameters in this function signature, we’re not changing the lifetimes of any values passed in or returned. Rather, we’re specifying that the borrow checker should reject any values that don’t adhere to these constraints.
In other words, the generic lifetime `'a` will get the concrete lifetime that is equal to the smaller of the lifetimes of `x` and `y`.

> The way in which you need to specify lifetime parameters depends on what your function is doing.

So far, we’ve only defined structs to hold owned types. It’s possible for structs to hold references, but in that case we would need to add a lifetime annotation on every reference in the struct’s definition.
We declare the name of the generic lifetime parameter inside angle brackets after the name of the struct so we can use the lifetime parameter in the body of the struct definition.

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}
```

### Lifetime elision

You have learned that every reference has a lifetime and that you need to specify lifetime parameters for functions or structs that use references. The Rust team found that Rust programmers were entering the same lifetime annotations over and over in particular situations. These situations were predictable and followed a few deterministic patterns. The developers programmed these patterns into the compiler’s code so the borrow checker could infer the lifetimes in these situations and wouldn’t need explicit annotations.

> The patterns programmed into Rust’s analysis of references are called the _lifetime elision rules_. Lifetimes on function or method parameters are called _input lifetimes_, and lifetimes on return values are called _output lifetimes_.

```rust
impl<'a> ImportantExcerpt<'a> {
    fn level(&self) -> i32 {
        3
    }
}
```

The lifetime parameter declaration after impl and its use after the type name are required, but we’re not required to annotate the lifetime of the reference to self because of the first elision rule.

Check [the Rustonomicon](https://doc.rust-lang.org/nomicon/lifetime-elision.html)

### The Static Lifetime

One special lifetime we need to discuss is `'static`, which means that this reference can live for the entire duration of the program. All string literals have the `'static` lifetime.

```rust
let s: &'static str = "I have a static lifetime.";
```
