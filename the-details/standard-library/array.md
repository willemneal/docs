---
description: A sequence of values of a generic type.
---

# Array

## API

### Static

* Array.**isArray**&lt;`U`&gt;\(value: `U`\): `bool` Tests if a value is an array.
* Array.**create**&lt;`T`&gt;\(capacity?: `i32`\): `Array<T>` Creates a new array with at least the specified capacity and length zero. Unlike `new Array`, this is safe for non-nullable references because it does not create a holey array.

### Constructor

* new **Array**&lt;`T`&gt;\(capacity?: `i32`\) Constructs a new array. Traps If `T` is a non-nullable reference type and `capacity`is greater than zero because the operation would create a holey array \(means: with `null` values\) conflicting with its non-nullable value type \(use `Array.create` instead\).

### Instance

* Array\#**length**: `i32` The length of this array. Setting the length to a value larger than internal capacity will automatically grow the array. Traps if `T` is of a non-nullable reference type and setting length would result in a holey array.
* Array\#**concat**\(other: `Array<T>`\): `Array<T>` Concatenates the elements of this and the other array to a new array, in this order.
* Array\#**copyWithin**\(target: `i32`, start: `i32`, end?: `i32`\): `this` Copies a region of an array's values over the respective values starting at the target location.
* Array\#**every**\(fn: `(value: T, index: i32, array: Array<T>) => bool`\): `bool` Calls the specified function with every value of the array until it finds the first value for which the function returns `false`. Returns `true` if all functions returned `true` or the array is empty, otherwise `false`.
* Array\#**fill**\(value: `T`, start?: `i32`, end?: `i32`\): `this` Replaces an array's values with the specified value from `start` to `end`.
* Array\#**filter**\(fn: `(value: T, index: i32, array: Array<T>) => bool`\): `Array<T>` Calls the specified function with every value of the array, returning a new array with all values for which the function returned `true`.
* Array\#**findIndex**\(fn: `(value: T, index: i32, array: Array<T>) => bool`\): `i32` 
* Array\#**forEach**\(fn: `(value: T, index: i32, array: Array<T>) => void`\): `void` 
* Array\#**includes**\(value: `T`, fromIndex?: `i32`\): `bool` 
* Array\#**indexOf**\(value: `T`, fromIndex?: `i32`\): `i32` 
* Array\#**join**\(separator?: `string`\): `string` 
* Array\#**lastIndexOf**\(value: `T`, fromIndex?: `i32`\): `i32` 
* Array\#**map**&lt;`U`&gt;\(fn: `(value: T, index: i32, array: Array<T>) => U`\): `Array<U>` 
* Array\#**pop**\(\): `T` 
* Array\#**push**\(value: `T`\): `i32` 
* Array\#**reduce**&lt;`U`&gt;\(fn: `Reducer`, initialValue: `U`\): `U` 
* Array\#**reduceRight**&lt;`U`&gt;\(fn: `Reducer`, initialValue: `U`\): `U` 
* Array\#**reverse**\(\): `Array<T>` 
* Array\#**shift**\(\): `T` 
* Array\#**slice**\(begin?: `i32`, end?: `i32`\): `Array<T>` 
* Array\#**some**\(fn: `(value: T, index: i32, array: Array<T>) => bool`\): `bool` 
* Array\#**sort**\(fn: `(a: T, b: T) => i32`\): `this` 
* Array\#**splice**\(start: `i32`, deleteCount?: `i32`\): `Array<T>` 
* Array\#**toString**\(\): `string` Returns the result of `Array#join()`.
* Array\#**unshift**\(value: `T`\): `i32` 

