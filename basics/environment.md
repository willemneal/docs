---
description: Reinventing the wheel. Again. But with a notch.
---

# Environment

A WebAssembly environment is more limited than a usual browser environment. AssemblyScript tries to fill the gaps by reimplementing commonly known functionality, besides providing direct access to WebAssembly instructions through built-ins.

## Standard library

AssemblyScript comes with its own standard library very much resembling what developers became used to when writing JavaScript.

{% page-ref page="../standard-library/globals.md" %}

{% page-ref page="../standard-library/array.md" %}

{% page-ref page="../standard-library/arraybuffer.md" %}

{% page-ref page="../standard-library/dataview.md" %}

{% page-ref page="../standard-library/date.md" %}

{% page-ref page="../standard-library/error.md" %}

{% page-ref page="../standard-library/map.md" %}

{% page-ref page="../standard-library/math.md" %}

{% page-ref page="../standard-library/number.md" %}

{% page-ref page="../standard-library/set.md" %}

{% page-ref page="../standard-library/string.md" %}

{% page-ref page="../standard-library/typedarray.md" %}

## Extended Library

The following specialized classes exist in AssemblyScript only, tackling very specific problems.

{% page-ref page="../extended-library/staticarray.md" %}

Additional rather low-level WebAssembly functionality that the standard library makes extensive use of is described below.

## Static type checks

By making use of the following special type checks, especially in generic contexts, untaken branches can be eliminated statically, leading to concrete WebAssembly functions that handle one type specificially.

* **isInteger**&lt;`T`&gt;\(value?: `T`\): `bool` Tests if the specified type _or_ expression is of an integer type and not a reference. Compiles to a constant.
* **isFloat**&lt;`T`&gt;\(value?: `T`\): `bool` Tests if the specified type _or_ expression is of a float type. Compiles to a constant.
* **isSigned**&lt;`T`&gt;\(value?: `T`\): `bool` Tests if the specified type _or_ expression can represent negative numbers. Compiles to a constant.
* **isReference**&lt;`T`&gt;\(value?: `T`\): `bool` Tests if the specified type _or_ expression is of a reference type. Compiles to a constant.
* **isString**&lt;`T`&gt;\(value?: `T`\): `bool` Tests if the specified type _or_ expression can be used as a string. Compiles to a constant.
* **isArray**&lt;`T`&gt;\(value?: `T`\): `bool` Tests if the specified type _or_ expression can be used as an array. Compiles to a constant.
* **isFunction**&lt;`T`&gt;\(value?: `T`\): `bool` Tests if the specified type _or_ expression is of a function type. Compiles to a constant.
* **isNullable**&lt;`T`&gt;\(value?: `T`\): `bool` Tests if the specified type _or_ expression is of a nullable reference type. Compiles to a constant.
* **isDefined**\(expression: `*`\): `bool` Tests if the specified expression resolves to a defined element. Compiles to a constant.
* **isConstant**\(expression: `*`\): `bool` Tests if the specified expression evaluates to a constant value. Compiles to a constant.
* **isManaged**&lt;`T`&gt;\(expression: `*`\): `bool` Tests if the specified type _or_ expression is of a managed type. Compiles to a constant. Usually only relevant when implementing custom collection-like classes.

### Example

```typescript
function add<T>(a: T, b: T): T {
  return a + b // addition if numeric, string concatenation if a string
}

function add<T>(a: T, b: T): T {
  if (isString<T>()) { // eliminated if T is not a string
    return parseInt(a) + parseInt(b)
  } else { // eliminated if T is a string
    return a + b
  }
}
```

{% hint style="info" %}
If you are not going to use low-level WebAssembly in the foreseeable future, feel free to come back to the following paragraphs at a later time and continue at the next page[ ](loader.md)right away.
{% endhint %}

## Sizes and alignments

* **sizeof**&lt;`T`&gt;\(\): `usize` Determines the byte size of the respective _basic type_. Means: If `T` is a class type, the size of `usize` is returned. To obtain the size of a class in memory, use `offsetof<T>()` instead. Compiles to a constant.
* **offsetof**&lt;`T`&gt;\(fieldName?: `string`\): `usize` Determines the offset of the specified field within the given class type. Returns the class type's end offset \(means: where the next field would be located, before alignment\) if field name has been omitted. Compiles to a constant. The `fieldName` argument must be a compile-time constant `string` because there is no information about field names anymore in the final binary. Hence, the field's name must be known at the time the returned constant is computed.
* **alignof**&lt;`T`&gt;\(\): `usize` Determines the alignment \(log2\) of the specified underlying _basic type_. Means: If `T` is a class type, the alignment of `usize` is returned. Compiles to a constant.

## Utility

* **assert**&lt;`T`&gt;\(isTrueish: `T`, message?: `string`\): `T` Traps if the specified value is not true-ish, otherwise returns the non-nullable value. Like assertions in C, aborting the entire program if the expectation fails, with the `--noAssert` option to disable all assertions in production.
* **instantiate**&lt;`T`&gt;\(...args: `*[]`\): `T` Instantiates a new instance of `T` using the specified constructor arguments.
* **changetype**&lt;`T`&gt;\(value: `*`\): `T` Changes the type of a value to another one. Useful for casting class instances to their pointer values and vice-versa.
* **idof**&lt;`T`&gt;\(\): `u32` Obtains the computed unique id of a class type. Usually only relevant when allocating objects or dealing with RTTI externally.
* **nameof**&lt;`T`&gt;\(value?: `T`\): `string` Determines the name of a given type.

## Low-level WebAssembly operations

### Math

The following generic built-ins compile to WebAssembly instructions directly.

* **clz**&lt;`T = i32 | i64`&gt;\(value: `T`\): `T` Performs the sign-agnostic count leading zero bits operation on a 32-bit or 64-bit integer. All zero bits are considered leading if the value is zero.
* **ctz**&lt;`T = i32 | i64`&gt;\(value: `T`\): `T` Performs the sign-agnostic count tailing zero bits operation on a 32-bit or 64-bit integer. All zero bits are considered trailing if the value is zero.
* **popcnt**&lt;`T = i32 | i64`&gt;\(value: `T`\): `T` Performs the sign-agnostic count number of one bits operation on a 32-bit or 64-bit integer.
* **rotl**&lt;`T = i32 | i64`&gt;\(value: `T`, shift: `T`\): `T` Performs the sign-agnostic rotate left operation on a 32-bit or 64-bit integer.
* **rotr**&lt;`T = i32 | i64`&gt;\(value: `T`, shift: `T`\): `T` Performs the sign-agnostic rotate right operation on a 32-bit or 64-bit integer.
* **abs**&lt;`T = i32 | i64 | f32 | f64`&gt;\(value: `T`\): `T` Computes the absolute value of an integer or float.
* **max**&lt;`T = i32 | i64 | f32 | f64`&gt;\(left: `T`, right: `T`\): `T` Determines the maximum of two integers or floats. If either operand is `NaN`, returns `NaN`.
* **min**&lt;`T = i32 | i64 | f32 | f64`&gt;\(left: `T`, right: `T`\): `T` Determines the minimum of two integers or floats. If either operand is `NaN`, returns `NaN`.
* **ceil**&lt;`T = f32 | f64`&gt;\(value: `T`\): `T` Performs the ceiling operation on a 32-bit or 64-bit float.
* **floor**&lt;`T = f32 | f64`&gt;\(value: `T`\): `T` Performs the floor operation on a 32-bit or 64-bit float.
* **copysign**&lt;`T = f32 | f64`&gt;\(x: `T` , y: `T`\): `T` Composes a 32-bit or 64-bit float from the magnitude of `x` and the sign of `y`.
* **nearest**&lt;`T = f32 | f64`&gt;\(value: `T`\): `T` Rounds to the nearest integer tied to even of a 32-bit or 64-bit float.
* **reinterpret**&lt;`T = i32 | i64 | f32 | f64`&gt;\(value: `*`\): `T` Reinterprets the bits of the specified value as type `T`. Valid reinterpretations are u32/i32 to/from f32 and u64/i64 to/from f64.
* **sqrt**&lt;`T = f32 | f64`&gt;\(value: `T`\): `T` Calculates the square root of a 32-bit or 64-bit float.
* **trunc**&lt;`T = f32 | f64`&gt;\(value: `T`\): `T` Rounds to the nearest integer towards zero of a 32-bit or 64-bit float.

### Memory

Similarly, the following built-ins emit WebAssembly instructions accessing or otherwise modifying memory.

* **load**&lt;`T`&gt;\(ptr: `usize`, immOffset?: `usize`\): `T` Loads a value of the specified type from memory. Equivalent to dereferencing a pointer in other languages.
* **store**&lt;`T`&gt;\(ptr: `usize`, value: `T`, immOffset?: `usize`\): `void` Stores a value of the specified type to memory. Equivalent to dereferencing a pointer in other languages when assigning a value.
* **memory.size**\(\): `i32` Returns the current size of the memory in units of pages. One page is 64kb.
* **memory.grow**\(value: `i32`\): `i32` Grows linear memory by a given unsigned delta of pages. One page is 64kb. Returns the previous size of the memory in units of pages or `-1` on failure. **Note** that calling `memory.grow`where a memory manager is present might break it.
* **memory.copy**\(dst: `usize`, src: `usize`, n: `usize`\): `void` Copies `n` bytes from `src` to `dst` . Regions may overlap. Emits the respective instruction if bulk-memory is enabled, otherwise ships a polyfill.
* **memory.fill**\(dst: `usize`, value: `u8`, n: `usize`\): `void` Fills `n` bytes at `dst` with the given byte `value`. Emits the respective instruction if bulk-memory is enabled, otherwise ships a polyfill.
* **memory.repeat**\(dst: `usize`, src: `usize`, srcLength: `usize`, count: `usize`\): `void`

  Repeat sequence of bytes from `src` with `srcLength` size `count` times in destination `dst`.

* **memory.compare**\(lhs: `usize`, rhs: `usize`, n: `usize`\): `i32`

  Compares the first `n` bytes of `left` and `rigth` and returns a value that indicates their relationship:  
  - **Negative** value if the first differing byte in `lhs` is less than the corresponding byte in `rhs`.  
  - **Positive** value if the first differing byte in `lhs` is greater than the corresponding byte in `rhs`.  
  - **Zero​** if all `n` bytes of `lhs` and `rhs` are equal.

The `immOffset` argument is a bit special here, because it becomes an actual immediate of the respective WebAssembly instruction instead of a normal operand. Thus it must be provided as a compile time constant value. This can be a literal or the value of a `const` variable that the compiler can precompute.

### **Memory Utility**

Sign-agnostic endian conversions \(reverse bytes\).

* **bswap**&lt;`T`&gt;\(value: `T`\): `T`

  Reverse all bytes for _**8-bit**_, _**16-bit**_, _**32-bit**_ and _**64-bit**_ integers.

* **bswap16**&lt;`T`&gt;\(value: `T`\): `T`

  Reverse only last 2 bytes regardless of the type of argument.



### Control flow

* **select**&lt;`T`&gt;\(ifTrue: `T`, ifFalse: `T`, condition: `bool`\): `T` Selects one of two pre-evaluated values depending on the condition. Differs from an `if/else` in that both arms are always executed and the final value is picked based on the condition afterwards. Performs better than an `if/else` only if the condition is random \(means: branch prediction is not going to perform well\) and both alternatives are cheap. It is also worth to note that Binaryen will do relevant optimizations like switching to a `select` automatically, so using a ternary `? :` for example is just fine.
* **unreachable**\(\): `*` Emits an unreachable instruction that results in a runtime error \(trap\) when executed. Both a statement and an expression of any type. Beware that trapping in managed code will most likely lead to memory leaks or even break the program because it ends execution prematurely.

### Atomics 🦄

The following instructions represent the [WebAssembly threads and atomics](https://github.com/WebAssembly/threads) specification. Must be enabled with `--enable threads`.

* atomic.**load**&lt;`T`&gt;\(ptr: `usize`, immOffset?: `usize`\): `T` Atomically loads an integer value from memory and returns it.
* atomic.**store**&lt;`T`&gt;\(ptr: `usize`, value: `T`, immOffset?: `usize`\): `void` Atomically stores an integer value to memory.
* atomic.**add**&lt;`T`&gt;\(ptr: `usize`, value: `T`, immOffset?: `usize`\): `T` Atomically adds an integer value in memory.
* atomic.**sub**&lt;`T`&gt;\(ptr: `usize`, value: `T`, immOffset?: `usize`\): `T` Atomically subtracts an integer value in memory.
* atomic.**and**&lt;`T`&gt;\(ptr: `usize`, value: `T`, immOffset?: `usize`\): `T` Atomically performs a bitwise AND operation on an integer value in memory.
* atomic.**or**&lt;`T`&gt;\(ptr: `usize`, value: `T`, immOffset?: `usize`\): `T` Atomically performs a bitwise OR operation on an integer value in memory.
* atomic.**xor**&lt;`T`&gt;\(ptr: `usize`, value: `T`, immOffset?: `usize`\): `T` Atomically performs a bitwise XOR operation on an integer value in memory.
* atomic.**xchg**&lt;`T`&gt;\(ptr: `usize`, value: `T`, immOffset?: `usize`\): `T` Atomically exchanges an integer value in memory.
* atomic.**cmpxchg**&lt;`T`&gt;\(ptr: `usize`, expected: `T`, replacement: `T`, immOffset?: `usize`\): `T` Atomically compares and exchanges an integer value in memory if the condition is met.
* atomic.**wait**&lt;`T`&gt;\(ptr: `usize`, expected: `T`, timeout: `i64`\): `AtomicWaitResult`  
  Performs a wait operation on an address in memory suspending this agent if the integer condition is met. Return values are

  | Value | Description |
  | :--- | :--- |
  | 0 | OK: Woken by another agent. |
  | 1 | NOT\_EQUAL: Loaded value did not match the expected value. |
  | 2 | TIMED\_OUT: Not woken before the timeout expired. |

* atomic.**notify**\(ptr: `usize`, count: `i32`\): `i32` Performs a notify operation on an address in memory waking up suspended agents.
* atomic.**fence**\(\): `void` Performs a fence operation, preserving synchronization guarantees of higher level languages.

Again, the `immOffset` argument must be a compile time constant value.

### SIMD 🦄

Likewise, these represent the [WebAssembly SIMD](https://github.com/WebAssembly/simd) specification. Must be enabled with `--enable simd`.

* **v128**\(a: `i8`, ... , p: `i8`\): `v128` Initializes a 128-bit vector from sixteen 8-bit integer values. Arguments must be compile-time constants.
* v128.**splat**&lt;`T`&gt;\(x: `T`\): `v128` Creates a vector with identical lanes.
* v128.**extract\_lane**&lt;`T`&gt;\(x: `v128`, idx: `u8`\): `T` Extracts one lane as a scalar.
* v128.**replace\_lane**&lt;`T`&gt;\(x: `v128`, idx: `u8`, value: `T`\): `v128` Replaces one lane.
* v128.**shuffle**&lt;`T`&gt;\(a: `v128`, b: `v128`, ...lanes: `u8[]`\): `v128` Selects lanes from either vector according to the specified lane indexes.
* v128.**swizzle**\(a: `v128`, s: `v128`\): `v128` Selects 8-bit lanes from the first vector according to the indexes \[0-15\] specified by the 8-bit lanes of the second vector.
* v128.**load**\(ptr: `usize`, immOffset?: `usize`, immAlign?: `usize`\): `v128` Loads a vector from memory.
* v128.**load\_splat**&lt;`T`&gt;\(ptr: `usize`, immOffset?: `usize`, immAlign?: `usize`\): `v128` Creates a vector with identical lanes by loading the splatted value.
* v128.**load\_ext**&lt;`TFrom`&gt;\(ptr: `usize`, immOffset?: `usize`, immAlign?: `usize`\): `v128` _integer only_ Creates a vector by loading the lanes of the specified type and extending each to the next larger type.
* v128.**store**\(ptr: `usize`, value: `v128`, immOffset?: `usize`, immAlign?: `usize`\): `void` Stores a vector to memory.
* v128.**add**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Adds each lane.
* v128.**sub**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Subtracts each lane.
* v128.**mul**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Multiplies each lane.
* v128.**div**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` _float only_ Divides each lane.
* v128.**neg**&lt;`T`&gt;\(a: `v128`\): `v128` Negates each lane.
* v128.**add\_saturate**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` _i8/i16 only_

  Adds each lane using saturation.

* v128.**sub\_saturate**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` _i8/i16 only_

  Subtracts each lane using saturation.

* v128.**shl**&lt;`T`&gt;\(a: `v128`, b: `i32`\): `v128` _integer only_

  Performs a bitwise left shift by a scalar on each lane.

* v128.**shr**&lt;`T`&gt;\(a: `v128`, b: `i32`\): `v128` _integer only_

  Performs a bitwise right shift by a scalar on each lane.

* v128.**and**\(a: `v128`, b: `v128`\): `v128`

  Performs the bitwise `a & b` operation on each lane.

* v128.**or**\(a: `v128`, b: `v128`\): `v128`

  Performs the bitwise `a | b` operation on each lane.

* v128.**xor**\(a: `v128`, b: `v128`\): `v128`

  Performs the bitwise `a ^ b` operation on each lane.

* v128.**andnot**\(a: `v128`, b: `v128`\): `v128` Performs the bitwise `!a & b` operation on each lane.
* v128.**not**\(a: `v128`\): `v128`

  Performs the bitwise `!a` operation on each lane.

* v128.**bitselect**\(a: `v128`, b: `v128`, mask: `v128`\): `v128`

  Selects bits of either vector according to the specified mask.

* v128.**any\_true**&lt;`T`&gt;\(a: `v128`\): `bool` Reduces a vector to a scalar indicating whether any lane is considered `true`.
* v128.**all\_true**&lt;`T`&gt;\(a: `v128`\): `bool` Reduces a vector to a scalar indicating whether all lanes are considered `true`.
* v128.**max**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Computes the maximum of each lane.
* v128.**min**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Computes the minimum of each lane.
* v128.**dot**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` _i16 only_ Computes the dot product of two lanes each, yielding lanes one size wider than the input.
* v128.**avgr**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128`\) _u8/u16 only_ Computes the rounding average of each lane.
* v128.**abs**&lt;`T`&gt;\(a: `v128`\): `v128` _float only_ Computes the absolute value of each lane.
* v128.**sqrt**&lt;`T`&gt;\(a: `v128`\): `v128` _float only_ Computes the square root of each lane.
* v128.**eq**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Computes which lanes are equal.
* v128.**ne**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Computes which lanes are not equal.
* v128.**lt**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Computes which lanes of the first vector are less than those of the second.
* v128.**le**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Computes which lanes of the first vector are less than or equal those of the second.
* v128.**gt**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Computes which lanes of the first vector are greater than those of the second.
* v128.**ge**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Computes which lanes of the first vector are greater than or equal those of the second.
* v128.**convert**&lt;`TFrom`&gt;\(a: `v128`\): `v128` _integer only_ Converts each lane from integer to floating point.
* v128.**trunc\_sat**&lt;`TTo`&gt;\(a: `v128`\): `v128` _integer only_ Truncates each lane from floating point to integer with saturation.
* v128.**narrow**&lt;`TFrom`&gt;\(a: `v128`, b: `v128`\): `v128` _integer only_ Narrows each lane to their respective narrower lanes.
* v128.**widen\_low**&lt;`TFrom`&gt;\(a: `v128`\): `v128` _integer only_ Widens the low lanes to their respective wider lanes.
* v128.**widen\_high**&lt;`TFrom`&gt;\(a: `v128`\): `v128` _integer only_ Widens the high lanes to their respective wider lanes.
* v128.**qfma**&lt;`T`&gt;\(a: `v128`, b: `v128`, c: `v128`\): `v128` _float only_ Computes `(a * b) + c` for each lane.
* v128.**qfms**&lt;`T`&gt;\(a: `v128`, b: `v128`, c: `v128`\): `v128` _float only_ Computes `(a * b) - c` for each lane.

In addition, the namespaces `i8x16`, `i16x8`, `i32x4`, `i64x2` , `f32x4` and `f64x2` provide their respective non-generic instructions, like `i32x4.splat` etc. Each of them can also be used to create a literal directly:

* **i8x16**\(a: `i8`, ... , p: `i8`\): `v128` Initializes a 128-bit vector from sixteen 8-bit integer values. Arguments must be compile-time constants.
* **i16x8**\(a: `i16`, ..., h: `i16`\): `v128` Initializes a 128-bit vector from eight 16-bit integer values. Arguments must be compile-time constants.
* **i32x4**\(a: `i32`, b: `i32`, c: `i32`, d: `i32`\): `v128` Initializes a 128-bit vector from four 32-bit integer values. Arguments must be compile-time constants.
* **i64x2**\(a: `i64`, b: `i64`\): `v128` Initializes a 128-bit vector from two 64-bit integer values. Arguments must be compile-time constants.
* **f32x4**\(a: `f32`, b: `f32`, c: `f32`, d: `f32`\): `v128` Initializes a 128-bit vector from four 32-bit float values. Arguments must be compile-time constants.
* **f64x2**\(a: `f64`, b: `f64`\): `v128` Initializes a 128-bit vector from two 64-bit float values. Arguments must be compile-time constants.

The namespaces `v8x16`, `v16x8`, `v32x4` and `v64x2` provide the respective sign agnostic instructions according to text format.

