# Variables and Data types

## Variable binding and Type inference

```rust
let foo = bar;
```

This line creates a new variable named `foo` and binds it to the value of the `bar` variable.

You don't need to define the type on assignment. But you must assign a value before usage. The compiler can usually
infer what type we want to use based on the value and how we use it.

```rust
let str; // declare str
str = "Hello, world!"; // assignement -> &str

let a = 40_000; // -> i32
let b = 40.; // -> f64
let v: [_; 5] = [3; 5];
//      ^ infers type for usage

let guess: u32 = "42".parse().expect("Not a number!");
```

The binding can be done at a later stage.

```rust
let a;
// ... some code
a = 42;
```

Of course, you can still specify the type manually.

```rust
let x: i32 = 45;
```

### Unused variables

While you are coding, you can say the compiler to not complain about a variable not being used.

```rust
let _unused_variable = 42;
```

### Throw away a value

During destructuring, if you don't want to deal with all values you can omit some with an underscore.
In use with the rest operator `..` it's very easy to just export what you need.

```rust
let _ = get_stuff(); // throws away the returned value

// The value 3 and the rest will not be assigned to a variable
let (_, b, ..) = (3, 7, 14, 45);
let (width, _) = get_size();
```

## Data types

The types covered here are all stored on the stack.

### Scalar Types

A _scalar_ type represents a single value. Rust has four primary scalar types: **integers**, **floating point numbers**
, **booleans** and **characters**.

```rust
let num: isize = 98_222;
let byt = b'A' // u8 only
let x = 2.0 // f64

let t: bool = true
```

When you are compiling in release mode, Rust does not checks for integers overflow. If you want to wrap explicitly, you
can use the standard library type `Wrapping`.

```rust
let zero = 0u8;

println!("{}", zero - 1_u8);
//             ^^^^^^^^^^^ attempt to compute `0_u8 - 1_u8`, which would overflow
```

```rust
# use std::num::Wrapping;
let zero = Wrapping(0u8);
let one = Wrapping(1u8);

println!("{}", (zero - one).0); // -> 255
```

Rust’s char type is four bytes in size and represents a Unicode Scalar Value, which means it can represent a lot more
than just ASCII. Accented letters; Chinese, Japanese and Korean characters; emoji; and zero-width spaces are all valid
char values in Rust.

```rust
let heart_eyed_cat = '😻';
```

### Compound Types

Rust has two primitive compound types: **tuples** and **arrays**.

#### The Tuple Type

This is a fixed length collections of values of different types.
We can access a tuple element directly bu using a `.` followed by the index of the value we want to access. The first
index in a tuple is `0`.

```rust
let tupl = ('x', 32); // => (char, i32)
let x = tupl.0; // => 'x'
let thirty_two = tupl.1; // => 32
```

As we will see in the "pattern" chapter, you can use _pattern matching_ to desctructure a tuple value.

```rust
let tup = (500, 6.4, 1);
let (x, y, z) = tup;
```

#### The Array Type

Arrays in Rust have a fixed length, like tuples. Arrays are useful when you want your data allocated on the stack rather
than the heap.
You can access elements of an array using indexing. As for tuple, the first index is `0`. If the index is greater than or
equal to the length, Rust will panic because of Rust's safety principle.

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];

let first = a[0];
```

> An array is not as flexible as the vector type. A vector (`Vec`) is a similar collection type provided by the standard
> library that is allowed to grow or shrink in size.

## Immutable by default

Mutability in Rust is explicit. When a variable le is immutable, once a value is bound to a name, you can’t change the
value.

```rust
struct Number { value: i32 }

let foo = 32;
foo = 40; // Nope

let n = Number {
    value: 3
};
n.value = 5; // Nope
n = Number { value: 5 } // Nope
```

Let's make it **mutable**. The entire instance must be mutable; Rust does not allow us to mark only certain fields as mutable.

```rust
struct Number { value: i32 }

let mut foo = 32;
foo = 40; // Yep

let mut n = Number {
    value: 3
};
n.value = 5; // Yep
n = Number { value: 5 } // Yep
```

### Mutable reference

> Of course this in not a good practice and a function should avoid to have side effect like this. But sometimes this is
> needed, like when you want to update a graphical context for example.

```rust
// This function will have side effects
fn change(a: &mut i32) {
    *a = 5
}

let mut a: i32 = 3;
change(&mut a); // Explicit mutability. You have been warned.

println!("{}", a); // => 5
```

## Constants

Constants are like immutable variables, but there are some differences:

- Declared using the const keyword.
- The type must be annotated. You can not infer it.
- You are not allowed to use mut with constants.
- Set only to a constant expression, not to the result of a function call or any other value that could only be computed
  at runtime.

```rust
const MAX_POINTS: u32 = 100_000;
```

## Shadowing

Shadowing is different from marking a variable as `mut`, because we will get a compile-time error if we accidentally try
to reassign to this variable without using the `let` keyword. By using let, we can perform a few transformations on a
value but have the variable be immutable after those transformations have been completed.

```rust
let x = 32;
let x = 32 + 8; // still immutable!

println!("{}", x); // => 40. Only the last x will be available
```

Even with a different type.

```rust
let x = 32;
let x = "Hey";

println!("{x}"); // => "Hey"
```
