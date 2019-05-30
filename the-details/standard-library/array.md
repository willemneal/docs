---
description: Represents a sequence of values of a generic type.
---

# Array

## API

### Static

* Array.**isArray**&lt;`U`&gt;\(value: `U`\): `bool` Tests if a value is an array.
* Array.**create**&lt;`T`&gt;\(capacity?: `i32`\): `Array<T>` Creates a new array with at least the specified capacity and length zero. Unlike `new Array`, this is safe for non-nullable references because it does not create a holey array.

### Constructor

* new **Array**&lt;`T`&gt;\(capacity?: `i32`\) Constructs a new array. Traps If `T` is a non-nullable reference type and `capacity`is greater than zero because the operation would create a holey array conflicting with its actual type \(use `Array.create` instead\).

### Instance

* Array\#**length**: `i32` The length of this array. Setting the length to a value larger than internal capacity will automatically grow the array. Traps if `T` is of a non-nullable reference type and setting length would result in a holey array.
* Array\#**concat**\(other: `Array<T>`\): `Array<T>` Merges the elements of this and the other array to a new array, in this order.
* Array\#copyWithin
* Array\#every
* Array\#fill
* Array\#filter
* Array\#findIndex
* Array\#forEach
* Array\#includes
* Array\#indexOf
* Array\#join
* Array\#lastIndexOf
* Array\#map
* Array\#pop
* Array\#push
* Array\#reduce
* Array\#reduceRight
* Array\#reverse
* Array\#shift
* Array\#slice
* Array\#some
* Array\#sort
* Array\#splice
* Array\#toString
* Array\#unshift

