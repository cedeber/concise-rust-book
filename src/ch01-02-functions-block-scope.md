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
