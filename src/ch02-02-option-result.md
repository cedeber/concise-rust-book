# Option and Result

## The billion dollars mistake

Rust does't have `null` or `nil`. It is replaced by the `Option` enumeration. The variants of `Option` if `Some` and `None`. The `None` variant represents no value while `Some` can hold one piece of data of any type.

Although some languages like _Kotlin_ or _Swift_ do enforce _strict null check_, as a code reader you never now if a value can be `null` or not. Again, in Rust, this is self-explanatory and explicit.

```rust
// As defined in Rust
enum Option<T> {
	None,
	Some(T),
}
```

```rust
let val1: Option<i32> = None;
let val2: Option<i32> = Some(32);

println!("{:?}, {:?}", val1, val2);
```

For instance in JavaScript (or TypeScript without the `strictNullCheck`)

```javascript
class Foo {
  name = "foo";

  bar() {
    console.log(this.name, "bar");
  }
}

const fb = Math.random() < 0.5 ? new Foo() : null;

fb.bar(); // => TypeError: null is not an object
```

As `null` doesn't exist in Rust, you need the `Option` type which forces you to check the nullity `None`;

```rust
use rand::prelude::*;

struct Foo {
	name: String,
}

impl Foo {
	fn new() -> Self {
		Foo { name: "foo".to_string() }
	}

	fn bar(&self) {
		println!("{} bar", self.name);
	}
}

let fb: Option<Foo> = if rand::random() {
	Some(Foo::new())
} else {
	None
};

fb.bar()
// ^^^ method not found in `Option<Foo>`
```

## Error management

There is no `try` / `catch`. `Result` is, like `Option`, also an enumeration. For `Result`, the variants are `Ok` and `Err`. The `Ok` variant indicates the operation was successful, and inside `Ok` is the successfully generated value. The `Err` variant means the operation failed, and `Err` contains information about how or why the operation failed.

The purpose of these `Result` which is `Ok(_)` or `Err(_)`.

> A function always returns a unit `()`, if nothing is specified.

```rust
use std::error::Error;

fn main() -> Result<(), Box<dyn Error>> {
	Ok(())
}
```

> ⚠️ Talk about `unwrap` and `?`
> If you don’t call `except`, the program will compile, but you will get a warning
