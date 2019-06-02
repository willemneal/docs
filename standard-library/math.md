---
description: Mathematical operations as known from JavaScript.
---

# Math

## Variants

Math in AssemblyScript is available in multiple variants.

| Variant | Description |
| :--- | :--- |
| NativeMath | WebAssembly implementation for `f64` |
| NativeMathf | WebAssembly implementation for `f32` |
| JSMath | JavaScript Math for `f64` imported from the host |

By default, `NativeMath` is aliased to become the global `Math` object and `NativeMathf` is aliased as `Mathf`, but the implementations can be changed through `--use Math=JSMath` for example. If `JSMath` is used, the host's `Math` object must be imported as `Math`. If `NativeMath` is used, the random number generator must be seeded with `NativeMath.seedRandom(seed: i64)` before the first use of `NativeMath.random()`.

{% hint style="warning" %}
The implementations of the trigonometric functions `sin`, `cos` and `tan` are not yet implemented for `NativeMath` and instead alias those of `JSMath` \(hence using them requires importing `Math`\), mostly because existing implementations are relatively large \(like utilizing relatively large lookup tables\) and we are still looking for smaller alternatives.
{% endhint %}

## API

The Math API is very much like JavaScript's \([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)\), with the notable exceptions stated above and rest parameters not being supported yet.

### Static

The type `T` below substitutes either `f32` or `f64` depending on the implementation used.

* Math.**E**: `T` _readonly_ The base of natural logarithms, e, approximately 2.718.
* Math.**LN2**: `T` _readonly_ The natural logarithm of 2, approximately 0.693.
* Math.**LN10**: `T` _readonly_ The natural logarithm of 10, approximately 2.302.
* Math.**LOG2E**: `T` _readonly_ The base 2 logarithm of e, approximately 1.442.
* Math.**LOG10E**: `T` _readonly_ The base 10 logarithm of e, approximately 0.434.
* Math.**PI**: `T` _readonly_ The ratio of the circumference of a circle to its diameter, approximately 3.14159.
* Math.**SQRT1\_2**: `T` _readonly_ The square root of 1/2, approximately 0.707.
* Math.**SQRT2**: `T` _readonly_ The square root of 2, approximately 1.414.
* Math.**abs**\(x: `T`\): `T` Returns the absolute value of `x`.
* Math.**acos**\(x: `T`\): `T` Returns the arccosine \(in radians\) of `x`.
* Math.**acosh**\(x: `T`\): `T` Returns the hyperbolic arc-cosine of `x`.
* Math.**asin**\(x: `T`\): `T` Returns the arcsine \(in radians\) of `x.`
* Math.**asinh**\(x: `T`\): `T` Returns the hyperbolic arcsine of `x`.
* Math.**atan**\(x: `T`\): `T` Returns the arctangent \(in radians\) of `x`.
* Math.**atan2**\(y: `T`, x: `T`\): `T` Returns the arctangent of the quotient of its arguments.
* Math.**atanh**\(x: `T`\): `T` Returns the hyperbolic arctangent of `x`.
* Math.**cbrt**\(x: `T`\): `T` eturns the cube root of `x`.
* Math.**ceil**\(x: `T`\): `T` Returns the smallest integer greater than or equal to `x`.
* Math.**clz32**\(x: `T`\): `T` Returns the number of leading zero bits in the 32-bit binary representation of `x`.
* Math.**cos**\(x: `T`\): `T` Returns the cosine \(in radians\) of `x`.
* Math.**cosh**\(x: `T`\): `T` Returns the hyperbolic cosine of `x`.
* Math.**exp**\(x: `T`\): `T` Returns e to the power of `x`.
* Math.**expm1**\(x: `T`\): `T` Returns e to the power of `x`, minus 1.
* Math.**floor**\(x: `T`\): `T` Returns the largest integer less than or equal to `x`.
* Math.**fround**\(x: `T`\): `T` Returns the nearest 32-bit single precision float representation of `x`.
* Math.**hypot**\(value1: `T`, value2: `T`\): `T` Returns the square root of the sum of squares of its arguments.
* Math.**imul**\(a: `T`, b: `T`\): `T` Returns the result of the C-like 32-bit multiplication of `a` and `b`.
* Math.**log**\(x: `T`\): `T` Returns the natural logarithm \(base e\) of `x`.
* Math.**log10**\(x: `T`\): `T` Returns the base 10 logarithm of `x`.
* Math.**log1p**\(x: `T`\): `T` Returns the natural logarithm \(base e\) of 1 + `x`.
* Math.**log2**\(x: `T`\): `T` Returns the base 2 logarithm of `x`.
* Math.**max**\(value1: `T`, value2: `T`\): `T` Returns the largest-valued number of its arguments.
* Math.**min**\(value1: `T`, value2: `T`\): `T` Returns the lowest-valued number of its arguments.
* Math.**pow**\(base: `T`, exponent: `T`\): `T` Returns `base` to the power of `exponent`.
* Math.**random**\(\): `T` Returns a pseudo-random number in the range from 0.0 inclusive up to but not including 1.0. When using a native implementation, see notes above on seeding the PRNG before first use.
* Math.**round**\(x: `T`\): `T` Returns the value of `x` rounded to the nearest integer.
* Math.**sign**\(x: `T`\): `T` Returns the sign of `x`, indicating whether the number is positive, negative or zero.
* Math.**signbit**\(x: `T`\): `bool` Returns whether the sign bit of `x` is set.
* Math.**sin**\(x: `T`\): `T` Returns the sine of `x`.
* Math.**sinh**\(x: `T`\): `T` Returns the hyperbolic sine of `x`.
* Math.**sqrt**\(x: `T`\): `T` Returns the square root of `x`.
* Math.**tan**\(x: `T`\): `T` Returns the tangent of `x`.
* Math.**tanh**\(x: `T`\): `T` Returns the hyperbolic tangent of `x`.
* Math.**trunc**\(x: `T`\): `T` Returns the integer part of `x` by removing any fractional digits.

## Considerations

The Math implementations are meant as a drop-in replacement and sometimes need to mimic special semantics from JavaScript, like `Math.round` always rounding towards `+Infinity`. Also, functions like `Math.fround` or `Math.imul`do not return an `f32` respectively an `i32` as some might expect for the same reason. Hence, depending on the use case, using [WebAssembly's math instructions](../basics/environment.md#math) directly can be a worthwhile alternative where portability is not a concern.

