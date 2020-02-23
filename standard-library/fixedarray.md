---
description: >-
  A randomly accessible sequence of values of a generic type with a fixed
  length.
---

# FixedArray

## API

The FixedArray API is similar to the [Array API](array.md), with the important difference that a FixedArray has a fixed length that cannot change. Unlike an Array, a FixedArray does not have a separate backing buffer, so no level of indirection, and as such can have performance characteristics very similar to arrays in C when used with `unchecked(...)`. As a side-effect, fixed arrays exclusively composed of static data compile to a single WebAssembly data segment.

### Constructor

* new **FixedArray**&lt;`T`&gt;\(length: `i32`\) Constructs a new fixed array.

### Static

* FixedArray.**fromArray**&lt;`T`&gt;\(source: `Array<T>`\): `FixedArray<T>` Creates a fixed array from a normal array.

### Instance

#### Fields

* FixedArray\#**length**: `i32` _readonly_

  The length of this array.

#### Methods

* FixedArray\#**concat**\(other: `Array<T>`\): `Array<T>` Concatenates the values of this and the other array to a new array, in this order.
* FixedArray\#**includes**\(value: `T`, fromIndex?: `i32`\): `bool` Tests if the array includes the specified value, optionally providing a starting index.
* FixedArray\#**indexOf**\(value: `T`, fromIndex?: `i32`\): `i32` Gets the first index where the specified value can be found in the array. Returns `-1` if not found.
* FixedArray\#**join**\(separator?: `string`\): `string` Concatenates all values of the array to a string, separated by the specified separator \(default: `,`\).
* FixedArray\#**lastIndexOf**\(value: `T`, fromIndex?: `i32`\): `i32` Gets the last index where the specified value can be found in the array. Returns `-1` if not found.
* FixedArray\#**slice**\(start?: `i32`, end?: `i32`\): `Array<T>` Returns a shallow copy of the array's values from `begin` inclusive to `end` exclusive, as a new array. If omitted, `end` defaults to the end of the array.
* FixedArray\#**toArray**\(\): `Array<T>` Creates a normal array from this fixed array.
* FixedArray\#**toString**\(\): `string` Returns the result of `FixedArray#join()`.

