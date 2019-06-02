---
description: A fixed-length sequence of UTF-16 code units.
---

# String

## API

The String API works very much like JavaScript's \([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)\), with the notable difference that the `string` type is an actual alias of `String`.

### Static

* String.**fromCharCode**\(unit: `i32`, surr?: `i32`\): `string` Creates a one character long string from the specified UTF-16 code units.
* String.**fromCharCodes**\(units: `u16[]`\): `string` _non-standard, todo?!_ Creates a string from a sequence of UTF-16 code units.
* String.**fromCodePoint**\(code: `i32`\): `string` Creates a one character long string from the specified UTF-8 code point.
* String.**fromCodePoints**\(codes: `i32[]`\): `string` _non-standard, todo?!_ Creates a string from a sequence of UTF-8 code points.

### Instance

* String\#**charAt**\(pos: `i32`\): `string` Gets the UTF-16 code unit at the specified position as a single character string. Returns `""` \(empty string\) if out of bounds.
* String\#**charCodeAt**\(pos: `i32`\): `i32` Gets the UTF-16 code unit at the specified position as a number. Returns `-1` if out of bounds.
* String\#**codePointAt**\(pos: `i32`\): `i32` Gets the UTF-8 code point at the specified \(UTF-16 code unit\) position as a number, possibly combining multiple successive UTF-16 code units. Returns `-1` if out of bounds.
* String\#**concat**\(other: `string`\): `string` Concatenates this string with another string, in this order, and returns the resulting string.
* String\#**endsWith**\(search: `string`, end?: `i32`\): `bool` Tests if the strings ends with the specified string. If specified, `end` indicates the position at which to stop searching, acting as if it is the length of the string.
* String\#**includes**\(search: `string`, start?: `i32`\): `bool` Tests if the string includes the search string. If specified, `start` indicates the position at which to begin searching.
* String\#**indexOf**\(search: `string`, start?: `i32`\): `i32` Gets the first index of the specified search string within the string, or `-1` if not found. If specified, `start` indicates the position at which to begin searching.
* String\#**lastIndexOf**\(search: `string`, start?: `i32`\): `i32` Gets the last index of the specified search string within the string, or `-1` if not found. If specified, `pos` indicates the position at which to begin searching from right to left.
* String\#**length**: `i32` _readonly_ The length of the string in UTF-16 code units.
* String\#**padStart**\(length: `i32`, pad: `string`\): `string` Pads the string with the contents of another string, possibly multiple times, until the resulting string reaches the specified length, returning the resulting string.
* String\#**padEnd**\(length: `i32`, pad: `string`\): `string` Pads the string with the contents of another string, possibly multiple times, until the resulting string reaches the specified length, returning the resulting string.
* String\#**repeat**\(count?: `i32`\): `string` Repeats the string `count` times and returns the concatenated result.
* String\#**slice**\(start: `i32`, end?: `i32`\): `string` Returns the region of the string from `start` inclusive to `end` exclusive, as a new string. If omitted, `end` defaults to the end of the string.
* String\#**split**\(separator?: `string`, limit?: `i32`\): `Array<string>` Splits the string at each occurrence  of `separator` and returns the result as a new array of strings that has a maximum of `limit` values. If `limit` is omitted, no limit is assumed. If `separator` is omitted or not present in the string, the string becomes the sole element of the array. If `separator` is an empty string, the split is performed between code units \(not code points\), potentially destroying surrogate pairs.
* String\#**startsWith**\(search: `string`, start?: `i32`\): `bool` Tests if the string starts with the specified string. If specified, `pos` indicates the position at which to begin searching, acting as the start of the string.
* String\#**substring**\(start: `i32`, end?: `i32`\): `string` Gets the part of the string in between `start` inclusive and `end` exclusive as a new string.
* String\#**toString**\(\): `this` Returns the string.
* String\#**trim**\(\): `string` Removes white space characters from both the start and the end of the string, returning the resulting string.
* String\#**trimStart**\(\): `string` _alias_  String\#**trimLeft** Removes white space characters from the start of the string, returning the resulting string.
* String\#**trimEnd**\(\): `string` _alias_  String\#**trimRight** Removes white space characters from the end of the string, returning the resulting string.

## UTF-16 vs UTF-8

Like JavaScript, AssemblyScript represents strings in [UTF-16 encoding represented by the API as UCS-2](https://mathiasbynens.be/notes/javascript-encoding), where certain UTF-8 **code points** are represented by two UTF-16 **code units**, so called surrogate pairs. This is done to mimic JavaScript as closely as possible, and in an attempt that calling JavaScript APIs from WebAssembly does not have to convert between encodings constantly. Chances are that some parts of the WebAssembly API [will settle for UTF-8](https://github.com/WebAssembly/webidl-bindings/blob/master/proposals/webidl-bindings/Explainer.md#binding-operators-and-expressions) exclusively, though, with [our objections](https://github.com/WebAssembly/webidl-bindings/issues/13) remaining unheard, and it is still to be decided how to proceed.

