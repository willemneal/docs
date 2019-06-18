---
description: A mapping of generic keys to generic values.
---

# Map

## API

The Map API is very similar to JavaScript's \([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)\), with the notable difference that a `.get` with a key that does not exist results in an error, because `undefined` cannot be represented. Example:

```typescript
var map = new Map<i32,string>();

// Because `undefined` cannot be represented if a key is not found, this will error:
var str = map.get(1); // ERROR

// The error can be avoided by first making sure that the key exists, so this works:
var str: string | null = map.has(1) ? map.get(1) : null; // OK
```

### Constructor

* new **Map**&lt;`K`,`V`&gt;\(\) Constructs a new map mapping keys of type `K` to values of type `V`.

### Instance

#### Fields

* Map\#**size**: `i32` _readonly_ The current number of key-value pairs in this map.

#### Methods

* Map\#**clear**\(\): `void` Clears the map, deleting all key-value pairs.
* Map\#**delete**\(key: `K`\): `bool` Deletes the key-value pair for the corresponding key. Returns `true` if the key did exist, otherwise `false`.
* Map\#**get**\(key: `K`\): `V` Gets the value corresponding to the specified key. Traps if the key does not exist because "not found" cannot be represented in all cases \(use `Map#has` to check\).
* Map\#**has**\(key: `K`\): `bool` Tests if the specified key exists.
* Map\#**keys**\(\): `Array<K>` Gets the keys contained in this map as an array, in insertion order. This is preliminary while iterators are not supported.
* Map\#**set**\(key: `K`, value: `V`\): void Sets the value for the specified key. Creates a new key-value pair if the key did not exist.
* Map\#**values**\(\): `Array<V>` Gets the values contained in this map as an array, in insertion order. This is preliminary while iterators are not supported.
* Map\#**toString**\(\): `string` Returns a string representation of this map.

