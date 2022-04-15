# Functions

Functions are pervasive in Rust code. The `main` function is the most important function in the language. It's the entry
point of many programs.

## Functions

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

In function signatures, you _must_ declare the type of each parameter

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

A semicolon is mandatory at the end of each expression. If it is _omitted_, it means a `return`.

> Take care of this one!

```rust
fn add_2(a: i32) -> i32 {
	a + 2
}

println!("{}", add_2(5)); // => 7
```

### Closure

```rust
let x = vec![1, 2, 3, 4]
    .iter()
    .map(|x| x + 3);

let closure_annotated = |i: i32| -> i32 { i + 1 };
let closure_inferred = |i | i + 1 ;
```

## Blocks and Scope

A pair of brackets declares a block, which has its own scope.

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

### Blocks are also expressions

They can evaluate to a value and can contain multiple statements.

```rust
let x = {
    let y = 1;
    let z = 2;
    y + z
};

println!("{x}"); // => 3
```

#### if conditionals

```rust
fn eight() -> i32 {
	if true {
		8
	} else {
		4
	}
}

println!("{}", eight()); // => 8
```

#### match pattern

```rust
use rand::prelude::\ *;

fn dice(cheat: bool) -> i32 {
	match cheat {
		true => 6,
		false => thread_rng().gen_range(0..=6),
	}
}

println!("{}", dice(true)); // => 6
```

## Rest and Destructuring

The rest operator `..` allows to fill the _holes_. Unlike the spread operator `...` in JavaScript, the rest
operator `..` comes at the end. In other words you are _not overwriting the values_, but you are **filling the missing
ones**.

The rest must be the last and not be followed by a comma.

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
