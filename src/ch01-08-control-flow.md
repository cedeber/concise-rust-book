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

Rust has three kinds of loops: `loop`, `while` and `for`.

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

It's often useful for a program to evaluate a condition within a loop. While the condition is true, the loop runs.

```rust
let mut number = 3;

while number != 0 {
	number = number - 1;
}
```

### Looping Through a Collection with `for`

You can use a `for` loop and execute some code for each item in a collection. The safety and conciseness of `for` loops make them the most commonly used loop construct in Rust.

```rust
for number in (1..4).rev() {
	println("{}!", number);
}
```

> `.rev()` reverses the range

### Loop label

If you have loops within loops, `break` and `continue` apply to the innermost loop at that point. You can optionally specify a _loop label_ on a loop that we can then use with `break` or `continue` to specify that those keywords apply to the labeled loop instead of the innermost loop.

```rust
let mut count = 0;

'counting_up: loop {
	let mut remaining = 10;

	loop {
		if remaining == 9 {
			break;
		}
		if count == 2 {
			break 'counting_up;
		}
		remaining -= 1;
	}

	count += 1;
}
```

## The match Control Flow Operator

Rust has an extremely powerful control flow operator called `match` that allows you to compare a value against a series of patterns and then execute code based on which pattern matches. The power of `match` comes from the expressiveness of the patterns and the fact that the compiler confirms that all possible cases are handled.

> When the `match` expression executes, it compares the resulting value against the pattern of each arm, in order.

The code associated with each arm is an expression, and the resulting value of the expression in the matching arm is the value that gets returned for the entire `match` expression.

> Curly brackets typically are not used if the match arm code is short.

Another useful feature of match arms is that they can bind to the parts of the values that match the pattern. This is how we can extract values out of enum variants.

Combining `match` and enums is useful in many situation. You will see this pattern a lot in Rust code: match against an enum, bind a variable to the data inside, and then execute code based on it.

```rust
struct Number {
	odd: bool,
	value: i32,
}

let one = Number { odd: true, value: 1 };
let two = Number { odd: false, value: 2 };

print_number(one); // => Odd number: 1
print_number(two); // => Even number: 2

// Same as with the if pattern
fn print_number(n: Number) {
	match n {
		Number { odd: true, value } => println!("Odd number: {}", value),
		Number { odd: false, value } => println!("Even number: {}", value),
	}
}
```

One requirement for `match` expressions is that they need to be _exhaustive_. At least one arm needs to match.
One way to ensure you have covered every possibility is to have a catchall pattern for the last arm. A particular pattern `_` will match anything, but it never binds to a variable.

```rust
fn print_number(n: Number) {
	match n.value {
		1 => println!("One"),
		2 => println!("Two"),
		_ => (),
	}
}
```

> The `()` is just the unit value, so nothing will happen in the `_` case here.

### Pattern syntax

#### Literals

```rust
1 => println!("one")
```

#### Named Variables

```rust
Some(x) => println!("x = {}", x)
```

#### Multiple

```rust
1 | 2 => println!("one or two")
```

#### Range with the ..= Syntax

Ranges are only allowed with numeric values or `char` values, because the compiler checks that the range isn’t empty at compile time.

```rust
1..=5 => println!("one through five"),
'a'..='j' => println!("early ASCII letter"),
```

### Match guard

A _match guard_ is an additional `if` condition specified after the pattern in a `match` arm that must also match, along with the pattern matching, for that arm to be chosen. Match guards are useful for expressing more complex ideas than a pattern alone allows.

```rust
fn main() {
    let num = Some(4);

    match num {
        Some(x) if x % 2 == 0 => println!("The number {} is even", x),
        Some(x) => println!("The number {} is odd", x),
        None => (),
    }
}
```

### @ Bindings

The `at` operator (`@`) lets us create a variable that holds a value at the same time we are testing that value to see whether it matches a pattern.

```rust
match msg {
	Message::Hello {
		id: id_variable @ 3..=7,
	} => println!("Found an id in range: {}", id_variable),
	Message::Hello { id: 10..=12 } => {
		println!("Found an id in another range")
	}
	Message::Hello { id } => println!("Found some other id: {}", id),
}
```

## Concise Control Flow with if let and while let

The `if let` syntax lets you combine `if` and `let` into a less verbose way to handle values that match one pattern while ignoring the rest.

> `if let` can also introduce shadowed variables in the same way that `match` arms can: `if let Ok(age) = age`.

```rust
let a = Some(40);
let b = None;

print_number(a); // => 40
print_number(b); // => No output

fn print_number(n: Option<i32>) {
	if let Some(value) = n {
		println!("{}", value)
	}
}
```

Similar in construction to `if let`, the `while let` conditional loop allows a while loop to run for as long as a pattern continues to match.

```rust
while let Some(top) = stack.pop() {
	println!("{}", top);
}
```
