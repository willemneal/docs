---
description: 'Let''s compile the uncompilable, shall we?'
---

# Introduction

AssemblyScript compiles a **strictly-typed** subset of [TypeScript](https://www.typescriptlang.org) \(a typed superset of JavaScript\) to [WebAssembly](https://webassembly.org) using [Binaryen](https://github.com/WebAssembly/binaryen), looking like:

{% code-tabs %}
{% code-tabs-item title="fib.ts" %}
```typescript
export function fib(n: i32): i32 {
  var t: i32, a: i32 = 0, b: i32 = 1;
  for (let i = 0; i < n; i++) {
    t = a + b; a = b; b = t;
  }
  return b;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

```text
$> asc fib.ts -b fib.wasm -O3
```

It's architecture differs from a JavaScript VM in that it compiles a program **ahead of time**, quite similar to other static compilers, while providing both low-level built-ins to access WebAssembly features directly as well as a JavaScript-like standard library on top of a relatively small managed runtime enabling the creation of programs that look and feel much like TypeScript.

For example, on the lowest level, memory can be accessed using the `load<T>(offset)` and `store<T>(offset, value)` built-ins that compile to WebAssembly instructions directly

```typescript
store<i32>(8, load<i32>(0) + load<i32>(4));
```

while the standard library provides implementations of `ArrayBuffer` , `Uint8Array` , `String` , `Map`etc. on a higher level.

```typescript
var view = new Int32Array(12);
view[2] = view[0] + view[1];
```

Certainly, the compiler is not finished yet, and there are still WebAssembly features undergoing specification \(marked as 🦄\) to make it shine, but it is open source and everyone can contribute. We are getting there.

Sounds appealing to you? Read on!

