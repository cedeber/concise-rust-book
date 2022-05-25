# Rust and JavaScript

## wasm-bindgen

In order to expose a Rust function to the outside world, you need to use the `#[wasm_bindgen]` macro.

```rust,noplayground
#[wasm_bindgen]
pub fn add(a: i32, b: i32) -> i32 {
	a + b
}
```

### JS Snippets

To include a local JS file, you'll use the `#[wasm_bindgen(module)]` macro.

#### Default with JS module

```javascript
// js/foo.js

export function add(a, b) {
  return a + b;
}
```

```rust,noplayground
#[wasm_bindgen(module = "/js/foo.js")]
extern "C" {
    fn add(a: u32, b: u32) -> u32;
}
```

#### Async with JS namespace

```javascript
window.__extern__ = {
	async_add: (a + b) => Promise.resolve(a + b);
	// or
	async_add: async (a + b) => a + b;
}
```

```rust,noplayground
#[wasm_bindgen(js_namespace = __extern__)]
extern "C" {
	#[wasm_bindgen(catch)]
	async fn async_add(a: u32, b: u32) -> Result<JsValue, JsValue>;
}
```

> You can also call any function declared in the `window` global scope but it is not recommended if you expose a lot of function.
