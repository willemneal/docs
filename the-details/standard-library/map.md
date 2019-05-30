---
description: Represents a mapping of generic keys to generic values.
---

# Map

## API

### Constructor

* new **Map**&lt;`K`,`V`&gt;\(\) Constructs a new map mapping keys of type `K` to values of type `V`.

### Instance

* Map\#**delete**\(key: `K`\): `bool` Deletes the key-value pair for the corresponding key. Returns `true` if the key did exist, otherwise `false`.
* Map\#**get**\(key: `K`\): `V` Gets the value corresponding to the specified key. Traps if the key does not exist because "not found" cannot be represented in all cases \(use `Map#has` to check\).
* Map\#**has**\(key: `K`\): `bool` Tests if the specified key exists.
* Map\#**keys**\(\): `Array<K>` Gets the keys contained in this map as an array, in insertion order. This is preliminary while iterators are not supported.
* Map\#**set**\(key: `K`, value: `V`\): void Sets the value for the specified key. Creates a new key-value pair if the key did not exist.
* Map\#**size**: `i32` _readonly_ The current number of key-value pairs in this map.
* Map\#**values**\(\): `Array<V>` Gets the values contained in this map as an array, in insertion order. This is preliminary while iterators are not supported.
* Map\#**toString**\(\): `string` Returns a string representation of this map.

