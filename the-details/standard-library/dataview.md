---
description: >-
  Provides a low-level interface for reading/writing number values from/to a raw
  binary ArrayBuffer.
---

# DataView

## API

### Constructor

* new **DataView**\(buffer: `ArrayBuffer`, byteOffset?: `i32`, byteLength?: `i32`\) Constructs a new DataView on the specified buffer and region.

### Instance

* DataView\#**buffer**: `ArrayBuffer` _readonly_ The backing buffer.
* DataView\#**byteLength**: `i32` _readonly_ The length of this view from the start of its buffer.
* DataView\#**byteOffset**: `i32` _readonly_ The offset of this view from the start of its buffer.
* DataView\#**getFloat32**\(byteOffset: `i32`, littleEndian?: `bool`\): `f32` Gets the 32-bit float value at the specified offset from the start of the view.
* DataView\#**getFloat64**\(byteOffset: `i32`, littleEndian?: `bool`\): `f64`

  Gets the 64-bit float value at the specified offset from the start of the view.

* DataView\#**getInt8**\(byteOffset: `i32`\): `i8` Gets the signed 8-bit integer value at the specified offset from the start of the view.
* DataView\#**getInt16**\(byteOffset: `i32`, littleEndian?: `bool`\): `i16` Gets the signed 16-bit integer value at the specified offset from the start of the view.
* DataView\#**getInt32**\(byteOffset: `i32`, littleEndian?: `bool`\): `i32`

  Gets the signed 32-bit integer value at the specified offset from the start of the view.

* DataView\#**getInt64**\(byteOffset: `i32`, littleEndian?: `bool`\): `i64`

  Gets the signed 64-bit integer value at the specified offset from the start of the view.

* DataView\#**getUint8**\(byteOffset: `i32`, littleEndian?: `bool`\): `u8`

  Gets the unsigned 8-bit integer value at the specified offset from the start of the view.

* DataView\#**getUint16**\(byteOffset: `i32`, littleEndian?: `bool`\): `u16`

  Gets the unsigned 16-bit integer value at the specified offset from the start of the view.

* DataView\#**getUint32**\(byteOffset: `i32`, littleEndian?: `bool`\): `u32`

  Gets the unsigned 32-bit integer value at the specified offset from the start of the view.

* DataView\#**getUint64**\(byteOffset: `i32`, littleEndian?: `bool`\): `u64` Gets the unsigned 64-bit integer value at the specified offset from the start of the view.
* DataView\#**setFloat32**\(byteOffset: `i32`, value: `f32`, littleEndian?: `bool`\): `void` Sets the 32-bit float value at the specified offset from the start of the view.
* DataView\#**setFloat64**\(byteOffset: `i32`, value: `f64`, littleEndian?: `bool`\): `void`

  Sets the 64-bit float value at the specified offset from the start of the view.

* DataView\#**setInt8**\(byteOffset: `i32`, value: `i8`\): `void` Sets the signed 8-bit integer value at the specified offset from the start of the view.
* DataView\#**setInt16**\(byteOffset: `i32`, value: `i16`, littleEndian?: `bool`\): `void` Sets the signed 16-bit integer value at the specified offset from the start of the view.
* DataView\#**setInt32**\(byteOffset: `i32`, value: `i32`, littleEndian?: `bool`\): `void`

  Sets the signed 32-bit integer value at the specified offset from the start of the view.

* DataView\#**setInt64**\(byteOffset: `i32`, value: `i64`, littleEndian?: `bool`\): `void`

  Sets the signed 64-bit integer value at the specified offset from the start of the view.

* DataView\#**setUint8**\(byteOffset: `i32`, value: `u8`, littleEndian?: `bool`\): `void`

  Sets the unsigned 8-bit integer value at the specified offset from the start of the view.

* DataView\#**setUint16**\(byteOffset: `i32`, value: `u16`, littleEndian?: `bool`\): `void`

  Sets the unsigned 16-bit integer value at the specified offset from the start of the view.

* DataView\#**setUint32**\(byteOffset: `i32`, value: `u32`, littleEndian?: `bool`\): `void`

  Sets the unsigned 32-bit integer value at the specified offset from the start of the view.

* DataView\#**setUint64**\(byteOffset: `i32`, value: `u64`, littleEndian?: `bool`\): `void` Sets the unsigned 64-bit integer value at the specified offset from the start of the view.
* DataView\#**toString**\(\): `string` Returns a string representation of this object.

Endianness defaults to `littleEndian = false`.

