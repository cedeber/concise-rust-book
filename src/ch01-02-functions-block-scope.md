# Functions, Blocks and Scope

A pair of brackets declares a block, which has its own scope. It can evaluate to a value and can contain multiple statements.

```rust
fn main() {
	let x = "out";
	{
		// this is a different `x`
		let x = "in";
		println!("{x}"); // => in
	}
	println!("{x}"); // => out
}
```

## Functions

Functions are pervasive in Rust code. The `main` function is the most important function in the language. It's the entry
point of many programs.

The `fn` keyword allows you to declare new functions. Rust code uses _snake case_ as the conventional style for
functions and variables names. Every function returns by default a **unit** type, written `()`. It's an empty tuple. No
need to specify it or return
it, it is implicit.

```rust
fn main() {
	// empty
}
```

> Rust does not care where you define your functions, only that they are defined somewhere.

### Parameters

In function signatures, you _must_ declare the type of each parameter.

```rust
fn another_function(x: i32, y: i32) {
	println!("x: {}", x);
	println!("y: {}", y);
}
```

### Return value

We do declare their type after an arrow `->`.

```rust
fn add(x: i32, y: i32) -> i32 {
	return x + y;
}
```

### Statements and Expressions in Function Bodies

Rust is an expression-based language. _Statements_ are instructions that perform some action and do not return a value. For instance, the `let y = 6` statement does not return a value. _Expressions_ evaluate to a resulting value. Calling a function is an expression. Calling a macro is an expression. The block that we use to create new scopes, `{}`, is an expression.

> Expressions do not include ending semicolons. If you add a semicolon to the end of an expression, you turn it into a statement, which will then not return a value.

```rust
fn add_2(a: i32) -> i32 {
	a + 2
}

println!("{}", add_2(5)); // => 7
```

## Closures

Rust's closures are anonymous functions you cqn save in a variable or pass as arguments to other functions.

To define a closure, we start with a pair if vertical pipes (`|`), inside which we specify the parameters to the closure. After the parameters, we place curly brackets that hold the body of the closure – these are optional if the closure body is a single expression.

Closures don't require you to annotate the types of the parameters or the return value like `fn` functions do. But as with variables, we can add type annotations of we want to increase explicitness and clarity at the cost of being more verbose than is strictly necessary.

```rust
let x = vec![1, 2, 3, 4]
    .iter()
    .map(|x| x + 3);

let closure_annotated = |i: i32| -> i32 { i + 1 };
let closure_inferred = |i | i + 1 ;
```

We can defined a closure and store the _closure_ in a variable rather than storing the result of the function call.

```rust
let expensive_closure = |num| {
	thread::sleep(Duration::from_secs(2));
	num
}
```

> The first time we call a closure with any value, the compiler infers the type of it and the return type of the closure. Those types are the locked into the closure and we get a type error if we try to use a different type with the same closure.

The `Fn` traits are provided by the standard library. All closures implement at least on the traits: `Fn`, `FnMut` or `FnOnce`.

### Capturing the Environment with Closures

Closures have an additional capability that functions don`t have: they can capture their environment and access variables from the scope in which they are defined.

```rust
let x = 4;
let equal_to_x = |z| z == x;
let y = 4;
assert!(equal_to_x(y));
```

Closures can capture values from their environment in three ways, which directly map to the three ways a function can take a parameter: taking ownership, borrowing mutably, and borrowing immutably. These are encoded in the three `Fn` traits.

<!-- prettier-ignore -->
| Trait    | Description |
| -------- | ----------- |
| `FnOnce` | consumes the variables it captures from its enclosing scope, known as closure's _environment_. To consume the captured variables, the closure must take ownership of these variables and move them into the closure when it is defined. The `Once` part represents the fact that the closure can not take ownership more than once, so it can be called only once. |
| `FnMut`  | can change the environment be it mutably borrows values |
| `Fn`     | borrows value from the environment immutability |

If you want to force the closure to take ownership of the values it uses in the environment, you can use the `move` keyword before the parameter list.

```rust
let equal_to_x = move |z| z == x;
```

## Iterators

The iterator pattern allows you to perform some task on a sequence of items in turn. An iterator is responsible for the logic of iterating over each item and determining when the sequence has finished.

In Rust, iterators are _lazy_, meaning they have no effect until you call methods that consume the iterator to use it up.

```rust
let v = vec![1, 2, 3];
let v_iter = v.iter();

for val in v_iter {
	// -- snip --
}
```

All iterators implement a trait named `Iterator` that is defined in the standard library.

```rust
pub trait Iterator {
	type Item;

	fn next(&mut self) -> Option<Self::Item>;

	// -- snip --
}
```

> Notice this definition uses some new syntax: `type Item` and `Self::Item`, which are defining an _associated type_ with this trait. The `Item` type will be the type returned from the iterator.

```rust
let v = vec![1, 2, 3];
let mut v_iter = v.iter();

assert_eq!(v_iter.next(), Some(&1));
assert_eq!(v_iter.next(), Some(&2));
assert_eq!(v_iter.next(), Some(&3));
assert_eq!(v_iter.next(), None);
```

We need to make iterator mutable: calling the `next` method on an iterator changes internal state that the iterator uses to keep track of where it is in the sequence.

We did not need to make `v_iter` mutable when we used a `for` loop because the lopp took ownership of `v_iter` and made it mutable behind the scenes.

The `iter` method produces an iterator over immutable references. If we want to create an iterator that takes ownership of `v` and returns owned values, we can call `into_iter` instead of `iter`. Similarly, if we want to iterate over mutable references, we can call `iter_mut` instead of `iter`.

### Creating our own Iterators

> The `Iterator` trait has a number of different methods with default implementations provided by the standard library. Other methods defined on the `Iterator`trait, known as _iterator adaptors_, allow you to change iterators into different kinds of iterators.

```rust
struct Counter {
	count: u32,
}

impl Counter {
	fn new() -> Self {
		Counter { count: 0 }
	}
}

impl Iterator for Counter {
	type Item = u32;

	fn next(&mut self) -> Option<Self::Item> {
		self.count += 1;

		if self.count < 6 {
			Some{self.count}
		} else {
			None
		}
	}
}
```

We implemented the `Iterator` trait by defining the `next` method, so we can now use any `Iterator` trait method's default implementations as defined in the standard library, because they all use the `next` method's functionality.

```rust
let sum: u32 = Counter::new().zip(Counter::new().skip(1))
							 .map(|(a, b)| a * b)
							 .filter(|x| x % 3 == 0)
							 .sum();
asset_eq!(sum, 18);
```
