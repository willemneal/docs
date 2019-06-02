---
description: An Array-like view on a raw binary buffer.
---

# TypedArray

## Variants

| Variant | Description |
| :--- | :--- |
| Int8Array | A view on 8-bit signed integer values of type `i8`. |
| Int16Array | A view on 16-bit signed integer values of type `i16`. |
| Int32Array | A view on 32-bit signed integer values of type `i32`. |
| Int64Array | A view on 64-bit signed integer values of type `i64`. |
| Uint8Array | A view on 8-bit unsigned integer values of type `u8`. |
| Uint8ClampedArray | Like Uint8Array but with set values of a larger type clamped between `0` and `255` inclusive instead of overflowing. |
| Uint16Array | A view on 16-bit unsigned integer values of type `u16`. |
| Uint32Array | A view on 32-bit unsigned integer values of type `u32`. |
| Uint64Array | A view on 64-bit unsigned integer values of type `u64`. |
| Float32Array | A view on 32-bit float values of type `f32`. |
| Float64Array | A view on 64-bit float values of type `f64`. |

## API

The TypedArray API is exactly like JavaScript's \([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)\). The name `TypedArray` below represents one of the variants named above of type `T` and is not an actual class.

### Static

* TypedArray.**BYTES\_PER\_ELEMENT**: `usize` _readonly_ Number of bytes per element.

### Constructor

* new **TypedArray**&lt;`T`&gt;\(length: `i32`\) Constructs a new typed array view with a new backing buffer and all values initialized to zero.

### Instance

#### Fields

* TypedArray\#**buffer**: `ArrayBuffer` _readonly_ The backing array buffer of this view.
* TypedArray\#**byteOffset**: `i32` _readonly_ The offset in bytes from the start of the backing buffer.
* TypedArray\#**byteLength**: `i32` _readonly_ The length in bytes from the start of the backing buffer.
* TypedArray\#**length**: `i32` _readonly_ The length in elements.

#### Methods

* TypedArray\#**fill**\(value: T, start?: i32, end?: i32\): this Replaces the values of the array from `start` inclusive to `end` exclusive in place with the specified value, returning the array.
* TypedArray\#**sort**\(fn: `(a: T, b: T) => i32`\): `this` Sorts the values of the array in place, using the specified comparator function, modifying the array before returning it. The comparator returning a negative value means `a < b`, a positive value means `a > b` and `0` means that both are equal. Unlike in JavaScript, where an implicit conversion to strings is performed, the comparator defaults to compare two values of type `T`.
* ...

