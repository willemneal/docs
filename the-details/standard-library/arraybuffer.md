---
description: A fixed-length raw binary data buffer.
---

# ArrayBuffer

## API

### Static

* ArrayBuffer.**isView**&lt;`T`&gt;\(value: `T`\): `bool` Returns true if `value` is one of the buffer views, such as one of the [typed arrays](typedarray.md) or a [DataView](dataview.md).

### Constructor

* new **ArrayBuffer**\(length: `i32`\) Constructs a new buffer of the given length in bytes.

### Instance

* ArrayBuffer\#**byteLength** The buffer's length, in bytes.
* ArrayBuffer\#**slice**\(begin?: `i32`, end?: `i32`\): `ArrayBuffer` Returns a copy of this buffer from begin, inclusive, up to end, exclusive.
* ArrayBuffer\#**toString**\(\):  `string` Returns a string representation of this buffer.

