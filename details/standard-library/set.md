---
description: A set of unique generic values.
---

# Set

## API

The Set API is very similar to JavaScript's \([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)\), but iterators are not implemented yet.

### Constructor

* new **Set**&lt;`T`&gt;\(\) Constructs a new set of unique value of type `T`.

### Instance

* Set\#**add**\(value: `T`\): `void` Adds the specified value to the set. Does nothing if the value already exists.
* Set\#**delete**\(value: `T`\): `bool` Deletes the specified value. Returns `true` if the value was found, otherwise `false`.
* Set\#**clear**\(\): `void` Clears the set, deleting all values.
* Set\#**has**\(value: `T`\): `bool` Tests if the specified value exists in the set.
* Set\#**values**\(\): `Array<T>` Gets the values contained in this set as an array, in insertion order. This is preliminary while iterators are not supported.
* Set\#**size**: `i32` _readonly_ The current number of unique values in the set.
* Set\#**toString**\(\): `string` Returns a string representation of this map.

