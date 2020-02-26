---
description: >-
  A randomly accessible sequence of values of a generic type with a fixed
  length.
---

# StaticArray

## API

The StaticArray API is similar to the [Array API](../standard-library/array.md), with the important difference that it has a fixed length that cannot change. Unlike a normal Array, a StaticArray does not have a separate backing buffer, so no level of indirection, and as such can have minimal overhead and performance characteristics very similar to arrays in C.

### Constructor

* new **StaticArray**&lt;`T`&gt;\(length: `i32`\) Constructs a new static array.

### Static

* StaticArray.**fromArray**&lt;`T`&gt;\(source: `Array<T>`\): `StaticArray<T>` Creates a static array from a normal array.
* StaticArray.**concat**&lt;`T`&gt;\(source: `StaticArray<T>`, other: `StaticArray<T>`\): `StaticArray<T>` Like `StaticArray#concat`, but taking and returning a `StaticArray`.
* StaticArray.**slice**&lt;`T`&gt;\(source: `StaticArray<T>`, start?: `i32`, end?: `i32`\): `StaticArray<T>` Like `StaticArray#slice`, but returning a `StaticArray`.

### Instance

#### Fields

* StaticArray\#**length**: `i32` _readonly_

  The fixed length of this static array.

#### Methods

* StaticArray\#**concat**\(other: `Array<T>`\): `Array<T>` Concatenates the values of this static and the other normal array to a new normal array, in this order.
* StaticArray\#**includes**\(value: `T`, fromIndex?: `i32`\): `bool` Tests if the array includes the specified value, optionally providing a starting index.
* StaticArray\#**indexOf**\(value: `T`, fromIndex?: `i32`\): `i32` Gets the first index where the specified value can be found in the array. Returns `-1` if not found.
* StaticArray\#**join**\(separator?: `string`\): `string` Concatenates all values of the array to a string, separated by the specified separator \(default: `,`\).
* StaticArray\#**lastIndexOf**\(value: `T`, fromIndex?: `i32`\): `i32` Gets the last index where the specified value can be found in the array. Returns `-1` if not found.
* StaticArray\#**slice**\(start?: `i32`, end?: `i32`\): `Array<T>` Returns a shallow copy of this static array's values from `begin` inclusive to `end` exclusive, as a new normal array. If omitted, `end` defaults to the end of the array.
* StaticArray\#**toString**\(\): `string` Returns the result of `StaticArray#join()`.

