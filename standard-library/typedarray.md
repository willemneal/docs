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
| Uint16Array | A view on 16-bit unsigned integer values of type `u16`. |
| Uint32Array | A view on 32-bit unsigned integer values of type `u32`. |
| Uint64Array | A view on 64-bit unsigned integer values of type `u64`. |
| Float32Array | A view on 32-bit float values of type `f32`. |
| Float64Array | A view on 64-bit float values of type `f64`. |
| Uint8ClampedArray | Like Uint8Array but with set values of a larger type clamped between `0` and `255` inclusive instead of overflowing. |

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

* TypedArray\#**every**\(fn: `(value: T, index: i32, self: TypedArray) => bool`\): `bool` Calls the specified function with every value of the array until it finds the first value for which the function returns `false`. Returns `true` if all functions returned `true` or the array is empty, otherwise `false`.
* TypedArray\#**fill**\(value: `T`, start?: `i32`, end?: `i32`\): `this` Replaces the values of the array from `start` inclusive to `end` exclusive in place with the specified value, returning the array.
* TypedArray\#**findIndex**\(fn: `(value: T, index: i32, self: TypedArray) => bool`\): `i32` Calls the specified function with every value of the array until it finds the first value for which the function returns `true`, returning its index. Returns `-1` if that's never the case.
* TypedArray\#**forEach**\(fn: `(value: T, index: i32, self: TypedArray) => void`\): `void` Calls the specified function with every value of the array.
* TypedArray\#**includes**\(value: `T`, fromIndex?: `i32`\): `bool` Tests if the array includes the specified value, optionally providing a starting index.
* TypedArray\#**indexOf**\(value: `T`, fromIndex?: `i32`\): `i32` Gets the first index where the specified value can be found in the array. Returns `-1` if not found.
* TypedArray\#**lastIndexOf**\(value: `T`, fromIndex?: `i32`\): `i32` Gets the last index where the specified value can be found in the array. Returns `-1` if not found.
* TypedArray\#**map**\(fn: `(value: T, index: i32, self: TypedArray) => T`\): `TypedArray` Calls the specified function with every value of the array, returning a new array of the function's return values.
* TypedArray\#**reduce**&lt;`U`&gt;\(fn: `Reducer`, initialValue: `U`\): `U` Calls the specified reducer function with each value of the array, resulting in a single return value. Reducer signature: `(acc: U, cur: T, idx: i32, src: Array) => U`.  The respective previous reducer function's return value is remembered in `acc`, starting with `initialValue`, becoming the final return value in the process.
* TypedArray\#**reduceRight**&lt;`U`&gt;\(fn: `Reducer`, initialValue: `U`\): `U` Calls the specified reducer function with each value of the array, from right to left, resulting in a single return value. See `Array#reduce` for the reducer function's signature.
* TypedArray\#**reverse**\(\): `this` Reverses an array's values in place, modifying the array before returning it.
* TypedArray\#**some**\(fn: `(value: T, index: i32, self: TypedArray) => bool`\): `bool` Calls the specified function with every value of the array until it finds the first value for which the function returns `true`, returning `true`. Returns `false` otherwise or if the array is empty.
* TypedArray\#**sort**\(fn: `(a: T, b: T) => i32`\): `this` Sorts the values of the array in place, using the specified comparator function, modifying the array before returning it. The comparator returning a negative value means `a < b`, a positive value means `a > b` and `0` means that both are equal. Unlike in JavaScript, where an implicit conversion to strings is performed, the comparator defaults to compare two values of type `T`.
* TypedArray\#**subarray**\(start?: `i32`, end?: `i32`\): `TypedArray` Returns a new view on the array's backing buffer from `begin` inclusive to `end` exclusive relative to this array. If omitted, `end` defaults to the end of the array. Does not copy.

