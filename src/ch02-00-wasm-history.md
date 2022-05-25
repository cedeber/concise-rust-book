# WebAssembly

> In this book, while talking about Wasm, I will target web browsers by default. If otherwise, it will be explicitely stated that we will target any or a specific runtime.

## Short History of Rust

- **2015**: Annonced
- **March 2017**: First release
- **November 2017**: Support "in all major browsers"
- **December 2019**: Recommendation from the W3C
- **2019**: Mozilla introduced its WebAssembly System Interface (WASI)
- **June 2019**: WebAssembly threads (Chrome 75)
- **May 2022**: 93% of installed browsers support WebAssembly

## Why WebAssembly?

Wasm (WebAssembly short name) is the little brother of asm.js. Wasm is a low-level assembly-like language with a compact binary format that runs with near-native performance. While the first implementations have landed in web browsers, there are also non-browser implementations for general-purpose use, including [Wasmtime](https://wasmtime.dev), [Wasmer](https://wasmer.io) or [WebAssembly Micro Runtime (WAMR)](https://github.com/bytecodealliance/wasm-micro-runtime), [wasm3](https://github.com/wasm3/wasm3), [WebAssembly Virtual Machine (WAVM)](https://github.com/WAVM/WAVM), and many others.
