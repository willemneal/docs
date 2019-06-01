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

{% hint style="warning" %}
The implementations of the trigonometric functions `sin`, `cos` and `tan` are not yet implemented for `NativeMath` and instead alias those of `JSMath` \(hence require importing `Math`\), mostly because existing implementations are relatively large \(like utilizing relatively large lookup tables\) and we are still looking for smaller alternatives.
{% endhint %}

## API

The Math API is very much like JavaScript's \([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)\), with the notable exceptions stated above and rest parameters not being supported yet.

* ...

