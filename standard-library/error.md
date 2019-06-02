---
description: Represents a runtime error.
---

# Error

## Kinds

| Kind | Description |
| :--- | :--- |
| Error | Represents a general error. |
| RangeError | Represents an error where a value is not in the range of allowed values. |
| TypeError | Represents an error where a value is not of the expected type. |
| SyntaxError | Represents an error where the syntax of the input in invalid. |

The `Error`class can also be sub-classed by forwarding `message` and setting the `name` property in the overloaded constructor.

## API

The Error API is very similar to JavaScript's \([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)\), with some properties not being implemented yet.

### Constructor

* new **Error**\(message?: `string`\) Constructs a new error object.

### Instance

* Error\#**message**: `string` The message of this error.
* Error\#**name**: `string` The name of this error. In case of `Error`, this is `"Error"`.
* Error\#**stack**: `string` The stack trace of this error. Not supported yet, hence an empty string.
* Error\#**toString**\(\): `string` Returns a string representation of this error in the form `name: message`.



