---
description: Reinventing the wheel. Again. But with a notch.
---

# Environment

WebAssembly runs in a sandbox, making its environment more limited than a usual browser environment. AssemblyScript tries to fill the gaps by reimplementing commonly known functionality, besides providing direct access to WebAssembly instructions through built-ins.

## Standard library

AssemblyScript comes with [its own standard library](../the-details/standard-library/) very much resembling what developers became used to when writing JavaScript. There is `ArrayBuffer`, the typed arrays, `String`, `Set`, `Map` and so on. Depending on how \(fast\) the relevant WebAssembly specifications for [reference types](https://github.com/WebAssembly/reference-types), [WebIDL bindings](https://github.com/WebAssembly/webidl-bindings) and [GC](https://github.com/WebAssembly/gc) develop, there might be additional features in the future, like importing parts of the standard library from the host. Additional rather low-level WebAssembly functionality that the standard library makes extensive use of is described below.

## Static type checks

By making use of statically evaluated type checks, especially in generic contexts, untaken branches can be eliminated statically, leading to concrete WebAssembly functions that handle one type specificially.

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

```typescript
function add<T>(a: T, b: T): T {
  return a + b; // addition if numeric, string concatenation if a string
}

function add<T>(a: T, b: T): T {
  if (isString<T>()) { // eliminated if T is not a string
    return parseInt(a) + parseInt(b);
  } else { // eliminated if T is a string
    return a + b;
  }
}
```

{% hint style="info" %}
If you are not going to use low-level WebAssembly in the foreseeable future, feel free to come back to the following paragraphs at a later time and continue at [Loader ](loader.md)right away.
{% endhint %}

* **sizeof**&lt;`T`&gt;\(\): `usize` Determines the byte size of the respective basic type. Compiles to a constant.
* **offsetof**&lt;`T`&gt;\(fieldName?: `string`\): `usize` Determines the offset of the specified field within the given class type. Returns the class type's end offset \(means: where the next field would be located, before alignment\) if field name has been omitted. Compiles to a constant. The `fieldName` argument must be a compile-time constant `string` because there is no information about field names anymore in the final binary. Hence, the field's name must be known at the time the returned constant is computed.
* **alignof**&lt;`T`&gt;\(\): `usize` Determines the alignment \(log2\) of the specified underlying core type. Compiles to a constant.

## Utility

* **assert**&lt;`T`&gt;\(isTrueish: `T`, message?: `string`\): `T` Traps if the specified value is not true-ish, otherwise returns the non-nullable value.
* **instantiate**&lt;`T`&gt;\(...args: `*[]`\): `T` Instantiates a new instance of `T` using the specified constructor arguments.
* **changetype**&lt;`T`&gt;\(value: `*`\): `T` Changes the type of a value to another one. Useful for casting class instances to their pointer values and vice-versa.
* **idof**&lt;`T`&gt;\(\): `u32` Obtains the computed unique id of a class type. Usually only relevant when allocating objects or dealing with RTTI externally.

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

The `immOffset` argument is a bit special here, because it becomes an actual immediate of the respective WebAssembly instruction instead of a normal operand. Thus it must be provided as a compile time constant value. This can be a literal or the value of a `const` variable that the compiler can precompute.

### Control flow

* **select**&lt;`T`&gt;\(ifTrue: `T`, ifFalse: `T`, condition: `bool`\): `T` Selects one of two pre-evaluated values depending on the condition. Differs from an `if/else` in that both arms are always executed and the final value is picked based on the condition afterwards. Performs better than an `if/else` only if the condition is random \(means: branch prediction is not going to perform well\) and both alternatives are cheap. It is also worth to note that Binaryen will do relevant optimizations like switching to a `select` automatically, so using a ternary `? :` for example is just fine.
* **unreachable**\(\): `*` Emits an unreachable operation that results in a runtime error when executed. Both a statement and an expression of any type.

### Calls

* **call\_indirect**&lt;`T`&gt;\(target: `u32`, ...args: `*[]`\): `T` Emits a `call_indirect` instruction, calling the specified function in the function table by index with the specified arguments. Does result in a runtime error if the arguments do not match the called function.

### Atomics 🦄

The following instructions represent the [WebAssembly threads and atomics](https://github.com/WebAssembly/threads) specification. Must be enabled with `--enable threads`.

* atomic.**load**&lt;`T`&gt;\(offset: `usize`, immOffset?: `usize`\): `T` Atomically loads an integer value from memory and returns it.
* atomic.**store**&lt;`T`&gt;\(offset: `usize`, value: `T`, immOffset?: `usize`\): `void` Atomically stores an integer value to memory.
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

Again, the `immOffset` argument must be a compile time constant value.

### SIMD 🦄

Likewise, these represent the [WebAssembly SIMD](https://github.com/WebAssembly/simd) specification. Must be enabled with `--enable simd`.

* **v128**\(a: `i8`, ... , p: `i8`\): `v128` Initializes a 128-bit vector from sixteen 8-bit integer values. Arguments must be compile-time constants.
* v128.**splat**&lt;`T`&gt;\(x: `T`\): `v128` Creates a 128-bit vector with identical lanes.
* v128.**extract\_lane**&lt;`T`&gt;\(x: `v128`, idx: `u8`\): `T` Extracts one lane from a 128-bit vector as a scalar.
* v128.**replace\_lane**&lt;`T`&gt;\(x: `v128`, idx: `u8`, value: `T`\): `v128` Replaces one lane in a 128-bit vector.
* v128.**shuffle**&lt;`T`&gt;\(a: `v128`, b: `v128`, ...lanes: `u8[]`\): `v128` Selects lanes from either 128-bit vector according to the specified lane indexes.
* v128.**load**\(offset: `usize`, immOffset?: `usize`, immAlign?: `usize`\): `v128` Loads a 128-bit vector from memory.
* v128.**store**\(offset: `usize`, value: `v128`, immOffset?: `usize`, immAlign?: `usize`\): `void` Stores a 128-bit vector to memory.
* v128.**add**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Adds each lane of two 128-bit vectors.
* v128.**sub**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Subtracts each lane of two 128-bit vectors.
* v128.**mul**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Multiplies each lane of two 128-bit vectors.
* v128.**div**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128` Divides each lane of two 128-bit vectors.
* v128.**neg**&lt;`T`&gt;\(a: `v128`\): `v128` Negates each lane of a 128-bit vector.
* v128.**add\_saturate**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128`

  Adds each lane of two 128-bit vectors using saturation.

* v128.**sub\_saturate**&lt;`T`&gt;\(a: `v128`, b: `v128`\): `v128`

  Subtracts each lane of two 128-bit vectors using saturation.

* v128.**shl**&lt;`T`&gt;\(a: `v128`, b: `i32`\): `v128`

  Performs a bitwise left shift on each lane of a 128-bit vector by a scalar.

* v128.**shr**&lt;`T`&gt;\(a: `v128`, b: `i32`\): `v128`

  Performs a bitwise right shift on each lane of a 128-bit vector by a scalar.

* v128.**and**\(a: `v128`, b: `v128`\): `v128`

  Performs the bitwise AND operation on each lane of two 128-bit vectors.

* v128.**or**\(a: `v128`, b: `v128`\): `v128`

  Performs the bitwise OR operation on each lane of two 128-bit vectors.

* v128.**xor**\(a: `v128`, b: `v128`\): `v128`

  Performs the bitwise XOR operation on each lane of two 128-bit vectors.

* v128.**not**\(a: `v128`\): `v128`

  Performs the bitwise NOT operation on each lane of a 128-bit vector.

* v128.**bitselect**\(a: `v128`, b: `v128`, mask: `v128`\): `v128`

  Performs the bitwise NOT operation on each lane of a 128-bit vector.  
  
  ...

\`\`

  


