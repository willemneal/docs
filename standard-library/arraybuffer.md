---
description: A fixed-length raw binary buffer.
---

# ArrayBuffer

## API

The ArrayBuffer API is exactly as in JavaScript \([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)\).

### Static

* ArrayBuffer.**isView**&lt;`T`&gt;\(value: `T`\): `bool` Returns true if `value` is one of the buffer views, such as one of the [typed arrays](typedarray.md) or a [DataView](dataview.md).

### Constructor

* new **ArrayBuffer**\(length: `i32`\) Constructs a new buffer of the given length in bytes.

### Instance

#### Fields

* ArrayBuffer\#**byteLength** The buffer's length, in bytes.

#### Methods

* ArrayBuffer\#**slice**\(begin?: `i32`, end?: `i32`\): `ArrayBuffer` Returns a copy of this buffer from begin, inclusive, up to end, exclusive.
* ArrayBuffer\#**toString**\(\):  `string` Returns a string representation of this buffer.

