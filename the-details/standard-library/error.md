---
description: Represents a runtime error.
---

# Error

## Kinds

* **Error** Represents a general error.
* **RangeError** extends **Error** Represents a range error.
* **TypeError** extends **Error** Represents a type error.
* **SyntaxError** extends **Error** Represents a syntax error.

The `Error`class can also be sub-classed by forwarding `message` and setting the `name` property in the overloaded constructor.

## API

### Constructor

* new **Error**\(message?: `string`\) Constructs a new error object.

### Instance

* Error\#**message**: `string` The message of this error.
* Error\#**name**: `string` The name of this error. In case of `Error`, this is `"Error"`.
* Error\#**stack**: `string` The stack trace of this error. Not supported yet, hence an empty string.
* Error\#**toString**\(\): `string` Returns a string representation of this error in the form `name: message`.



