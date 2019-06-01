---
description: Mathematical operations as known from JavaScript.
---

# Math

## Flavors

Math in AssemblyScript is available in multiple flavors.

| Implementation |  |
| :--- | :--- |
| NativeMath | WebAssembly implementation for `f64` |
| NativeMathf | WebAssembly implementation for `f32` |
| JSMath | JavaScript Math for `f64` imported from the host |

By default, `NativeMath` is aliased to become the global `Math` object and `NativeMathf` is aliased as `Mathf`, but the implementations can be changed through `--use Math=JSMath` for example. If `JSMath` is used, the host's `Math` object must be imported as `Math`. If `NativeMath` is used, the random number generator must be seeded with `NativeMath.seedRandom(seed: i64)` before the first use of `NativeMath.random()`.

## API

The Math API is very much like JavaScript's \([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)\).

* ...

