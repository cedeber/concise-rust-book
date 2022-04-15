# The Rust Programming Language

## Short history of Rust

Rust began, in 2006, as a side project of Graydon Hoare, an employee at Mozilla. Mozilla believed in the potential of
this new language and began sponsoring it. It was revealed to the world in 2010.

According to Hoare, the name of the language comes from the rust fungus. This has caused Rust programmers to adopt
“Rustaceans” as their nickname of choice.

In mid-2020, following the layoffs at Mozilla, the Rust core team said they were considering a new Rust foundation to
boost Rust independence. It was officially announced in February 2021.

### Editions

- Rust 2015: `1.0` (cargo’s `edition` has been introduced in 2017)
- Rust 2018: `1.31` or `edition="2018"`
- Rust 2021: `1.56` or `edition="2021"`

## Why Rust?

Rust is a very nice language. It has been designed to cover most development areas, from an easy CLI application to
system programming. And all of that with **safety**, **control** and **productivity** in mind.

Regarding safety, you can think of Python, and for control, C. Rust is some kind of mix of the best of both of these
worlds. While I do not think Rust is a good replacement for Python as such, it is for sure a perfect replacement for C
or any low-level languages, with the safety and fun of Python, but without trading the **performance** of C-like
languages at the same time.

A more personal point of view, what I like too, is the fact that Rust is an **explicit** language. There is no magic.
What you read if what it is going to happen. There are no tricks, like for instance in JavaScript, where Array and
Object are passed by reference. If you don't know that, you will have weird side effects. In Rust you always know what
happens, even with mutability. You will definitely code with **confidence**.

> The Rust programming language is fundamentally about empowerment: no matter what kind of code you are writing, Rust
> empowers you to reach further, to program with confidence in a wide variety of domains than you did before.
>
> — _Foreword from the Rust programming language book._

In the other hand, it makes things a little bit more complex and **noisy**.
Well, this is the reason of this book and why Rust is known to have a steep learning curve.

### What makes it different to most of common programming languages?

- It does **not have a garbage collector**. But you can still choose how the memory is managed. And you do not have to
  free the memory yourself. It has a lot of smart pointers for **memory efficiency**.
- Because of how memory is managed - the **ownership** principle - it is **memory and thread safe**. There are no
  dangling pointers, no data race, no buffer overflow, no iterator invalidation.
- It keeps **performance**, very close to what C can offer. Even sometimes faster.
- No `null` pointer exceptions.
- No `class` and so, no `inheritance`.
- First-class **WebAssembly** support.

### Ecosystem

Last but not least, although not related to the Rust language itself, the whole ecosystem and tooling around Rust is
just awesome. It is definitely feature-complete:

- A complete ecosystem (`rustup`, `cargo`, `crates`, `rustfmt`, `clippy`, ...)
- Good IDE support (IntelliJ-based and VSCode)
- Documentation directly in source code, from the comments
- Examples folder managed with `cargo`
- Tests in source code and `cargo test`
- `cargo` can be expanded: `cargo-edit`, `cargo-watch`, ...
- Compiler giving hints thanks to `clippy`
