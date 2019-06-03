---
description: Wrappers for basic numerical values.
---

# Number

The `Number` object has been split into one class per basic WebAssembly type as well. Unlike in JavaScript, these classes cannot have actual instances.

{% hint style="warning" %}
It is planned to pick up the instance members described below automatically, for example when concatenating a string with a number, but this is not yet functional.
{% endhint %}

## API

The Number API works a bit different than JavaScript's \([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)\).

### Integers

The name `Number` below stands for one of `I8`, `I16`, `I32`, `I64`, `U8`, `U16`, `U32` or `U64` representing their respective basic type `T`.

#### Static

* Number.**MIN\_VALUE**: `T` _readonly_ The smallest representable value within the respective basic type. This differs from floating point values where this is the smallest representable positive value.
* Number.**MAX\_VALUE**: `T` _readonly_ The largest representable value within the respective basic type.
* Number.**parseInt**\(value: `string`, radix?: `i32`\): `T` Parses a string to a value of the respective basic type.

#### Instance

* Number\#**toString**\(radix?: i32\): `string` Returns the respective basic value converted to a string. The `radix` parameter is not supported yet.

### Floats

The name `Number` below stands for one of `F32` or `F64` representing their respective basic type `T`.

#### Static

* Number.**EPSILON**: `T` _readonly_ The difference between `1.0` and the smallest number larger than `1.0`.
* Number.**MAX\_VALUE**: `T` _readonly_ The largest representable positive value within the respective basic type.
* Number.**MIN\_VALUE**: `T` _readonly_ The smallest representable positive value within the respective basic type.
* Number.**MAX\_SAFE\_INTEGER**: `T` _readonly_ The largest safe integer representable by the respective basic type.
* Number.**MIN\_SAFE\_INTEGER**: `T` _readonly_ The smallest safe integer representable by the respective basic type.
* Number.**POSITIVE\_INFINITY**: `T` _readonly_ Positive infinity within the respective basic type.
* Number.**NEGATIVE\_INFINITY**: `T` _readonly_ Negative infinity within the respective basic type.
* Number.**NaN**: `T` _readonly_ NaN \(Not A Number\) in the respective basic type.
* Number.**isNan**\(value: `T`\): `bool` Tests if the value is `Number.NaN`.
* Number.**isFinite**\(value: `T`\): `bool` Tests if the value is finite, that is not `Number.NaN` , `Number.POSITIVE_INFINITY` or `Number.NEGATIVE_INFINITY.`
* Number.**isInteger**\(value: `T`\): `bool` Tests if the value is an integer.
* Number.**isSafeInteger**\(value: `T`\): `bool` Tests if the value is a safe integer.
* Number.**parseInt**\(value: `string`, radix?: `i32`\): `T` Parses a string to an integer value of the respective basic type.
* Number.**parseFloat**\(value: `string`\): `T` Parses a string to a float value of the respective basic type.

#### Instance

* Number\#**toString**\(radix?: `i32`\): `string` Returns the respective basic value converted to a string. The `radix` parameter is not supported yet.



