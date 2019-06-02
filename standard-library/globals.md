---
description: Non-class elements in the global scope.
---

# Globals

The following global constants and functions are present alongside the standard library's classes.

* **NaN**: `f32 | f64` Not a number as a 32-bit or 64-bit float depending on context. Compiles to a constant.
* **Infinity**: `f32 | f64`  Positive infinity as a 32-bit or 64-bit float depending on context. Compiles to a constant.
* **isNaN**&lt;`f32 | f64`&gt;\(value: `T`\): `bool` Tests if a 32-bit or 64-bit float is `NaN`.
* **isFinite**&lt;`f32 | f64`&gt;\(value: `T`\): `bool` Tests if a 32-bit or 64-bit float is finite, that is not `NaN` or +/-`Infinity`.
* **parseInt**\(str: `string`, radix?: `i32`\): `i64` Parses a string to a 64-bit integer. Returns `0` on invalid inputs.
* **parseFloat**\(str: `string`\): `f64` Parses a string to a 64-bit float. Returns `NaN` on invalid inputs. This is a naive implementation currently.

