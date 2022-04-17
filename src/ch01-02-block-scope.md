# Blocks and Scope

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

Rust is an expression-based languag. _Statements_ are instructions that perform some action and do not return a value. For instance, the `let y = 6` statement does not return a value. _Expressions_ evaluate to a resulting value. Calling a function is an expression. Calling a macro is an expression. The block that we use to create new scopes, `{}`, is an expression.

> Expressions do not include ending semicolons. If you add a semicolon to the end of an expression, you turn it into a statement, which will then not return a value.

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

## Control Flow

### `if` Expressions

Blocks of code associated with the conditions in `if` expressions are sometimes called _arms_, just like the arm in `match` expressions. Optionally, we can also include an `else` expression (or `else if`). It's also worth noting that the condition _must_ be a `bool`.

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

#### Using `if` in a `let` statement

Because `if` is an expression, we can use it on the right side of a `let` statement. Remember that blocks of code evaluate to the last expression in them. This means the values that have the potential to be results from each arm of the `if` must be the **same type**.

```rust
let condition = true;
let number = if condition {
	5
} else {
	6
};

println!(number is "{}", number);
```

### Repetitions

Rust has thre kinds of loops: `loop`, `while` and `for`.

#### Loops

```rust
loop {
	println!("again!");
}
```

You might need to pass the result of a `loop`. To do this, add the value you want returned after the `break` expression, you use to stop the loop.

```rust
let mut counter = 0;

let result = loop {
	counter += 1;

	if counter == 10 {
		break counter * 2;
	}
};
```

#### Conditional Loops with `while`

It's often useful for a program to evaluate a condition within a loop. While the condition is true, le loop runs.

```rust
let mut number = 3;

while number != 0 {
	number = number - 1;
}
```

### Looping Through a Collection with `for`

You can use a `for` loop and execute some code for each item in a collection. The safety and consiness of `for` loops make them the most commonly used loop construct in Rust.

```rust
for number in (1..4).rev() {
	println("{}!", number);
}
```

> `.rev()` reverses the range
