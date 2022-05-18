# Closures and Iterators

## Closures

Rust's closures are anonymous functions you can save in a variable or pass as arguments to other functions.

```rust
let x = vec![1, 2, 3, 4]
    .iter()
    .map(|x| x + 3);
```

To define a closure, we start with a pair of vertical pipes (`|`), inside which we specify the parameters to the closure. After the parameters, we place curly brackets that hold the body of the closure – these are optional if the closure body is a single expression.

Closures don't require you to annotate the types of the parameters or the return value like `fn` functions do. But as with variables, we can add type annotations of we want to increase explicitness and clarity at the cost of being more verbose than is strictly necessary.

```rust
let closure_annotated = |i: i32| -> i32 { i + 1 };
let closure_inferred = |i | i + 1 ;
```

We can define a closure and store the _closure_ in a variable rather than storing the result of the function call.

The first time we call a closure with any value, the compiler infers the type of it and the return type of the closure. Those types are the locked into the closure, and we get a type error if we try to use a different type with the same closure.

### Capturing the Environment with Closures

Closures have an additional capability that functions don`t have: they can capture their environment and access variables from the scope in which they are defined.

```rust
let x = 4;
let equal_to_x = |z| z == x;
let y = 4;
assert!(equal_to_x(y));
```

Closures can capture values from their environment in three ways, which directly map to the three ways a function can take a parameter: taking ownership, borrowing mutably, and borrowing immutably. These are encoded in the three `Fn` traits.

> The `Fn` traits are provided by the standard library. All closures implement at least one of the traits: `Fn`, `FnMut` or `FnOnce`.

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

Notice this definition uses some new syntax: `type Item` and `Self::Item`, which are defining an _associated type_ with this trait. The `Item` type will be the type returned from the iterator.

```rust
let v = vec![1, 2, 3];
let mut v_iter = v.iter();

assert_eq!(v_iter.next(), Some(&1));
assert_eq!(v_iter.next(), Some(&2));
assert_eq!(v_iter.next(), Some(&3));
assert_eq!(v_iter.next(), None);
```

We need to make iterator mutable: calling the `next` method on an iterator changes internal state that the iterator uses to keep track of where it is in the sequence.

We did not need to make `v_iter` mutable when we used a `for` loop because the loop took ownership of `v_iter` and made it mutable behind the scenes.

The `iter` method produces an iterator over immutable references. If we want to create an iterator that takes ownership of `v` and returns owned values, we can call `into_iter` instead of `iter`. Similarly, if we want to iterate over mutable references, we can call `iter_mut` instead of `iter`.

### Creating our own Iterators

The `Iterator` trait has a number of different methods with default implementations provided by the standard library. Other methods defined on the `Iterator`trait, known as _iterator adaptors_, allow you to change iterators into different kinds of iterators.

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
