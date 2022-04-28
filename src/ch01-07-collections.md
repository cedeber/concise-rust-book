# Collections

Unlike the built-in array and tuple types, the data these collections point to is stored on the heap, which means the amount of data does not need to be known at compile time.

## Vectors

Vectors `Vec<T>` allow you to store more than on value in a single data structure that puts all the values next to each other in memory. Vectors can only store values of the same type `T`.

```rust
let v: Vec<i32> = Vec::new();
v.push(1);
// ...
// or
let v = vec![1, 2, 3];
```

> Rust provides the `vec!`macro for convenience.

### Reading Elements of Vectors

There are two ways to reference a value stored in a vector.

```rust
let v = vec![1, 2, 3, 4, 5];

let third: &i32 = &v[2];

match v.get(2) {
    Some(third) => println!("The third element is: {}.", third);
    None => println!("There is no third element.");
}
```

> The first `[]` method may cause the program to panic if it references a nonexistent element, like `&v[100]`.
> When the `get` method is passed an index that is outside the vector, it returns `None` without panicking.

When the program has a valid reference, the borrow checker enforces the ownership and borrowing rules to ensure this reference and any other references to the contents of the vector remain valid.

```rust
let first = &v[0];
//           - immutable borrow occurs here
v.push(6);
//^^^^^^^ mutable borrow occurs here
println!("The first element is: {}", first);
//                                   ----- immutable borrow later used here
```

This error is due to the way vectors work: adding a new element onto the end of the vector might require allocating new memory and copying the old elements to the new space. In that case, the reference to the first element would be pointing to deallocated memory.

### Iterating over the Values in a Vector

If we want to access each element in a vector in turn, we can iterate through all of the elements rather than use indices to access one at a time.

We can also iterate over mutable references to each element in a mutable vector in order to make changes to all the elements.

```rust
let mut v = vec![100, 32, 57];
for i in &mut v {
    *i += 50;
}
```

> To change the value that the mutable reference refers to, we have to use the dereference operator `*` to get to the value in `i` before we can use the `+=` operator.

### Using an Enum to Store Multiple Types

We said that vectors can only store values that are the same type. Fortunately, the variant of an enum are defined under the same enum type, so when we need to store elements of a different type in vector, we can defined and use an enum.

```rust
enum SpreadsheetCell {
    Int(i32),
    Float(f64),
    Text(String),
}

let row = vec![
    SpreadsheetCell::Int(3),
    SpreadsheetCell::Text(String::from("blue")),
    SpreadsheetCell:: Float(10.12),
];
```

## Storing UTF-8 Encoded Text with Strings

Rust has only one string type in the core language, which is the string slice `str` that is usually seen in its borrowed form `&str`.

The `String` type, which is provided by Rust`s standard library rather than coded into the core language, is a growable, mutable, owned, UTF-8 encoded string type.

```rust
let mut s = String::new();
s.push_str("hello");
let h = s[0];
```

```rust
let s1 = String::from("tic");
let s1 = String::from("tac");
let s1 = String::from("toe");

let s = s1 + "-" + &s2 + "-" + &s3; // note s1 has been moved here
let s = format!("{}-{}-{}", s1, s2, s3);
```

> A `String` is a wrapper over a `Vec<u8>`.

In UTF-8, each Unicode scalar value may takes more than one byte of storage. Think of Cyrillic, Japanese or Hindi signs. Therefore, an index into the string's bytes will not always correlate to a valid Unicode scalar value.

Another point about UTF-8 is that there are actually three relevant ways to look at strings from Rust's perspective: as bytes, scalar values, and grapheme clusters (the closest thing to what we would call _letters_).

Sometimes when you extract chars from a string, you will have diacritics that do not make sense on their own. So, indexing into a string is often a bad idea because it is not clear what the return type of the string-indexing operation should be: a byte value, a character, a grapheme cluster, or a string slice.

Getting grapheme clusters from strings is complex, so this functionality is not provided by the standard library. You can check the [`unicode-segmentation`](https://crates.io/crates/unicode-segmentation) crate.

> Rust has chosen to make the correct handling of `String` data the default behaviour for all Rust programs, which means programmers have to put more thought into handling UTF-8 data up front. This trade-off exposes more of the complexity of strings than is apparent in other programming languages, but it prevents you from having to handle errors involving non-ASCII characters later in your development life cycle

## Hash Maps

The type `HashMap<K, V>` stores a mapping of keys of type `K` to values of type `V`.

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);
```

> Of our three common collections, this one is the least often used, so it is not included in the features brought into scope automatically in the prelude.

Another way of constructing a hash map is by using the `collect` method on a vector of tuples, where each tuple consists of a key and its value.

```rust
let map = vec![(String::from("Blue"), 10), (String::from("Yellow"), 50)].collect();
```

We could also use the `zip` method to create a vector of tuples.

```rust
use std::collections::HashMap;

let teams = vec![String::from("Blue"), String::from("Yellow")];
let initial_scores = vec![10, 50];

let scores: HashMap<_, _> = teams.iter().zip(initial_scores.iter()).collect();
```

> For types that implements the `Copy`trait, such as `i32`, the values are copied into the hash map. For owned values such as `String`, the values will be moved and the hash map will be the owner of those values.

### Accessing Values

We can get a value out of the hash map by proving its key to the `get` method. The result is wrapped in `Some` because `get` returns an `Option<&V>`.

We can iterates over each key/value pair in a similar manner as we do with vectors, using a `for` loop.

```rust
for (key, value) in &scores {
    println!("{}: {}", key, value);
}
```

## Updating a Hash Map

> Each key can only have on value associated with it at a time.

If we insert a key and a value into a hash map and then insert that same key with a different value, the value associated with that key will be replaced.

Hash Maps have a special API for inserting a value only if the key has no value, called `entry`. It takes the key you want to check as a parameter. The return value of the entry method is an enum called `Entry` that represents a value that might or might not exist.

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);

scores.entry(String::from("Yellow")).or_insert(50);
scores.entry(String::from("Blue")).or_insert(50);
```

The `or_insert` method on `Entry` is defined to return a mutable reference to the value for the corresponding `Entry` key if that exists, and if not, inserts the parameter as the new value for this key and returns a mutable reference (`&mut V`) to the new value.

> By default, `HashMap` uses a cryptographically strong hashing function that is not the fastest algorithm available. You can switch to another function by specifying a different _hasher_. A hasher is a type that implements the `BuildHasher`trait.
