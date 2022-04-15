# Ownership, Borrowing and Lifetime

Probably the most known particularities of Rust. To sum it up very quickly: Every value is owned by one single variable,
during a single lifetime.

> Don’t try to fight against the borrow checker.

This first example will fail as name has been passed to `_p` and is not owned by `name` anymore.

```rust
struct Person {
	first_name: String,
}

fn main() {
	let name = String::from("Cédric");
	//  ---- move occurs because `name` has type `String`,
	//       which does not implement the `Copy` trait

	let _p = Person { first_name: name };
	//                            ---- value moved here

	println!("{}", name);
	//             ^^^^ value borrowed here after move
}
```

It can be borrowed, though. For that you need to use references and lifetime tags.

The & indicates that this argument is a reference, which gives you a way to let multiple parts of your code access one
piece of data without needing to copy that data into memory multiple times. References are a complex feature, one of
Rust’s major advantages is how safe and easy it is to use references. References passed to Person<'a> will now be alive
as long as its usage, which is the implicit lifetime of main().

```rust
struct Person<'a> {
	first_name: &'a str,
}

fn main() {
	let name = String::from("Cédric");
	let _p = Person { first_name: &name };

	println!("{}", name);
}
```

Or simply copy or clone the value. Copy is a shallow copy, while Clone is a deep clone.

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

implement Copy/Clone trait
