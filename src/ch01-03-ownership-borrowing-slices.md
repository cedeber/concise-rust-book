# Ownership, Borrowing and Slices

Ownership is the most unique feature of Rust, and it enables Rust to make memory safety guarantees without needing a garbage collector.

> Don’t try to fight against the borrow checker.

## The Stack and the Heap

The _stack_ stores values in the order it gets them and removes the values in the opposite order. This is referred to as _last in, first out_. All data stored in the stack must have a known, fixed size. Data with an unknown size at compile time or a size that might change must be stored on the heap instead.

The _heap_ is less organised. The operating system finds an empty spot in the heap that is big enough, marks it as being in use, and returns a pointer, which is the address of that location. This process is called _allocating on the heap_.

Pushing to the stack is faster than allocating on the heap because the operating system never has to search for a place to store new data. Comparatively, allocating space on the heap requires more work, because the operating system must first find a big enough space to hold the data and then perform bookkeeping to prepare for the next allocation.

Accessing data in the heap is slower than accessing data on the stack because you have to follow a pointer to get there. Contemporary processors are faster if they jump around less in memory.

## Memory and Allocation

As an example, there are two types of strings in Rust: `String` and `&str`.

- A `String` is stored as a vector of bytes (`Vec<u8>`), but guaranteed to always be a valid UTF-8 sequence. String is heap allocated, growable and not null terminated. With the `String` type, in order to support a mutable, growable piece of text, we need to allocate an amount of memory on the heap, unknown at compile time, to hold the content.
- `&str` is a slice (`&[u8]`) that always points to a valid UTF-8 sequence, and can be used to view into a `String`, just like `&[T]` is a view into `Vec<T>`. In the case of a string literal, we know the content at compile time, so the text is hardcoded directly into the final executable. This is why string literals are fast and efficient.

```rust
let mut s = String::From("hello");

s.push_str(", world!"); // appends a literal to a String
```

### Drop

When a variable goes out of scope, Rust calls a special function for us. This function is called `drop`. Rust call `drop` automatically at the closing curly bracket.

### Move

```rust
let x = 5;
let y = x;
```

> bind the value 5 to `x`; then make a **copy** of the value in `x` and bind it to `y`.

This is indeed what is happening, because integers are simple values with a known, fixed size and these two 5 values are pushed on the **stack**.

```rust
let s1 = String::from("hello");
let s2 = s1;
```

A `String` is made of three parts: a _pointer_ to the memory that holds the content of the string, a _length_, and a _capacity_.

<div style="display: flex; gap: 40px; justify-content: center;">
<div>
<p style="text-align: center;">s1</p>

| name     | value |
| -------- | ----- |
| ptr      | →     |
| len      | 5     |
| capacity | 5     |

</div>
<div>
<p style="text-align: center;">data</p>

| index | value |
| ----- | ----- |
| 0     | h     |
| 1     | e     |
| 2     | l     |
| 3     | l     |
| 4     | o     |

</div>
</div>

When we assign `s1` to `s2`, the `String` data is copied, meaning we copy the pointer, the length, and the capacity that are on the **stack**.
_We do not copy the data on the heap that the pointer refers to_.

Again, when a variable goes out of scope, Rust automatically calls the `drop` function and cleans up the heap memory for that variable.
When s2 and s1 go out of scope, they will both try to free the same memory. This is known as a _double free_ error and is one of the memory safety bugs.

Instead of trying to copy the allocated memory, Rust considers s1 to no longer be valid and, therefore, Rust doesn't need to free anything when s1 goes out of scope.

If you’ve heard the terms _shallow copy_ and _deep copy_ while working with other languages, the concept of copying the pointer, length, and capacity without copying the data probably sounds like making a shallow copy. But because Rust also invalidates the first variable, instead of calling it a shallow copy, it’s known as a _move_.

> Rust will never automatically create “deep” copies of your data. Therefore, any _automatic_ copying can be assumed to be inexpensive in terms of runtime performance.

### Clone

```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {}, s2 = {}", s1, s2);
```

When you see a call to `clone`, you know that some arbitrary code is being executed and that code may be expensive. It’s a visual indicator that something different is going on.

### Stack-Only Data: Copy

Types such as integers that have a known size at compile time are stored entirely on the stack, so copies of the actual values are quick to make.
In other words, there’s no difference between deep and shallow copying here, so calling clone wouldn’t do anything different from the usual shallow copying and we can leave it out.

> Rust has a special annotation called the `Copy` trait that we can place on types that are stored on the stack like integers are.

Here are some of the types that implement Copy:

- All the integer types, such as `u32`.
- The Boolean type, `bool`, with values `true` and `false`.
- All the floating point types, such as `f64`.
- The character type, `char`.
- Tuples, if they only contain types that also implement `Copy`.

### Ownership and Functions

Passing a variable to a function will `move` or `copy`, just as assignment does.
The ownership of a variable follows the same pattern every time: assigning a value to another variable moves it.

---

## Ownership rules

- Each value in Rust has a variable that's called its _owner_.
- There can be only one owner at a time.
- When the owner goes out of scope, the value will be dropped.

This first example will fail as name has been passed to `Person.first_name`, bound to `p` and is not owned by `name` anymore.

```rust
struct Person {
	first_name: String,
}

fn main() {
	let name = String::from("Cédric");
	//  ---- move occurs because `name` has type `String`,
	//       which does not implement the `Copy` trait

	let p = Person { first_name: name };
	//                            ---- value moved here

	println!("{}", name);
	//             ^^^^ value borrowed here after move
}
```

It can be borrowed, though. For that you need to use references and lifetime tags.

The `&` indicates that this argument is a reference, which gives you a way to let multiple parts of your code access one
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

---

## References and Borrowing

```rust
fn main() {
	let s1 = String::from("hello");
	let len = calculate_length(&s1);
	println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
	s.len()
}
```

These ampersands are _references_, and they allow you to refer to some value without taking ownership of it.

> The opposite of referencing by using `&` is _deferencing_, which is accomplished with the dereference operator `*`.

Just as variables are immutable by default, so are references. But mutable references have one big restriction: you can have only one mutable reference to a particular piece of data in a particular scope.

```rust
let r1 = &mut s;
//       ------ first mutable borrow occurs here
let r2 = &mut s;
//       ^^^^^^ second mutable borrow occurs here

println!("{}, {}", r1, r2);
//                 -- first borrow later used here
```

The benefit of having this restriction is that Rust can prevent data races at compile time. As always, we can use curly brackets to create a new scope, allowing for multiple mutable references, just not _simultaneous_ ones.

A similar rule exists for combining mutable and immutable references. We also cannot have a mutable reference while we have an immutable one.

```rust
let r1 = &s;
//       -- immutable borrow occurs here
let r2 = &s;
let r3 = &mut s;
//       ^^^^^^ mutable borrow occurs here

println!("{}, {}, and {}", r1, r2, r3);
//                         -- immutable borrow later used here
```

### Dangling references

In language with pointers, it's easy to erroneously create a _dangling pointer_, a pointer that references a location in memory that have been given to someone else or freed. In Rust, if you have a reference to some data, the compiler will ensure that the data will not go out of scope before the reference to the data does.

```rust
fn main() {
	let ref_to_nothing = dangle();
}

fn dangle() -> &String {
	let s = String::from("hello");

	&s
}
```

Because `s` is created inside `dangle`, when the code of `dangle` is finished, `s` will be deallocated. But we tried to return a reference to it. That means this reference would be pointing to an invalid `String`.

### The Slice Type

Another data type that does not have ownership is the _slice_. Slices let you reference a continuous sequence of elements in a collection rather than the whole collection.

A _string slice_, for instance, is a reference to part of a `String`.

```rust
let s = String::from("hello world");
let hello = &s[0..5];
let world = &s[6..11];
```

With Rust's `..` range syntax,

- if you want to start at the first index (zero), you can drop the trailing number: `[..5]`
- if your slice includes the last byte of the `String`, you can drop the trailing number: `[6..]`
- you can also drop both values to take a slice of the entire `String`: `[..]`

Recall from the borrowing rules that if we have an immutable reference to something, we cannot also take a mutable reference. In the example bellow, because `clear` needs to truncate the `String`, it tries to take a mutable reference, which fails.

```rust
let word = first_word(&s)
//                    -- immutable borrow occurs here
s.clear();
//^^^^^^^ mutable borrow occurs here
println!("the first word is: {}", word);
//                                ---- immutable borrow later used here
```

#### Other Slices

- String literals are slices
- Arrays are a general slice type, like `&[i32]`

### The Rules of References

- At any given time, you can have _either_ one mutable reference _or_ any number of immutable references.
- References must always be valid.
