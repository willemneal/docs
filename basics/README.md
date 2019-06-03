---
description: 'There is something appealing to it, isn''t it?'
---

# The Basics

Unlike TypeScript, which targets a JavaScript environment with all of its dynamic features, AssemblyScript targets WebAssembly, and intentionally **avoids the dynamicness of JavaScript** where it cannot be compiled ahead of time _efficiently_.

The first thing one is going to notice is that AssemblyScript's [type system](types.md) differs from TypeScript's in that it uses WebAssembly's **more specific integer and floating point types**, with `number` merely an alias of `f64`. On the WebAssembly level, these types are enough to implement other numerical types and higher level structures like strings and arrays as provided by the [standard library](environment.md#standard-library), yet the relevant specifications \(like [reference types](https://github.com/WebAssembly/reference-types) and [GC](https://github.com/WebAssembly/gc)\) to describe higher level structures to JavaScript are not yet available.

This means that **objects cannot yet flow in and out of WebAssembly natively**, making it necessary to read/write them from/to memory. To make this process more convenient, [the loader](loader.md) provides the utility necessary to translate between the WebAssembly and the JavaScript world.

It must also be noted that WebAssembly runs in a sandbox with **no immediate access to the DOM** or other JavaScript APIs currently, sometimes making it necessary to create custom glue code.

Combined, this makes it **unlikely that existing TypeScript code can be compiled** to WebAssembly without modifications, yet likely that already **reasonably strict TypeScript code can be made compatible** with the AssemblyScript compiler.

But what does this mean _exactly_? Well, let there be...

## Differences

For a better understanding of what to expect, let's start with a collection of obviously "not so strictly typed" code snippets, and how to fix them.

### Strict typing

There is no `any` or `undefined` for obvious reasons:

```typescript
// 😢
function foo(a?) {
  var b = a + 1;
  return b;
}

// 😊
function foo(a?: i32 = 0): i32 {
  var b = a + 1;
  return b;
}
```

There are no union types:

```typescript
// 😢
function foo(a: i32 | string): void {
}

// 😊
function foo<T>(a: T): void {
}
```

Objects must be strictly typed as well:

```typescript
// 😢
var a = {};
a.prop = "hello world";

// 😊
var a = new Map<string,string>();
a.set("prop", "hello world");

// 😏
class A {
  constructor(public prop: string) {}
}
var a = new A("hello. world");
```

### The case of ===

AssemblyScript uses `===` for identity comparisons \(means: the exact same object\). Idea is that its special JavaScript semantics for strings \(same type and value\) become irrelevant in a strict type context anyway. Some like this better, some hate it for portability reasons.

```typescript
var s1 = "1";
var s2 = "123".substring(0, 1);
s1 === s1 // true
s1 === s2 // false
s2 === s2 // true
s1 === 1 // compile error
s1 == s2 // true
```

For all other types than `string` and operator-overloaded objects with a `==` overload, `===` is equivalent to `==`.

```typescript
1 === 1 // true
1 == 1 // true
obj === obj // true
obj == obj // true if there is no == overload doing something different
```

### Imports

With [WebAssembly ES Module Integration](https://github.com/WebAssembly/esm-integration) still in the pipeline, imports utilize the ambient context currently. For example

{% code-tabs %}
{% code-tabs-item title="env.ts" %}
```typescript
export declare function doSomething(foo: i32): void;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

creates an import of a function named `doSomething` within the `env` module, because that's the name of the file it lives is. It is also possible to use namespaces:

{% code-tabs %}
{% code-tabs-item title="foo.ts" %}
```typescript
declare namespace console {
  export function logi(i: i32): void;
  export function logf(f: f64): void;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

This will import the functions `console.logi` and `console.logf` within the `foo` module. Bonus: Don't forget `export`ing namespace members if you'd like to call them from outside the namespace.

Where automatic naming is not sufficient, the `@external` decorator can be used to give an element another external name:

{% code-tabs %}
{% code-tabs-item title="bar.ts" %}
```typescript
@external("doSomethingElse")
export declare function doSomething(foo: i32): void;
// imports bar.doSomethingElse as doSomething

@external("foo", "baz")
export declare function doSomething(foo: i32): void;
// imports foo.baz as doSomething
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Conclusion

Even when considering that there are other [technical limitations](limitations.md) currently, this still means that it is pretty much possible already to create working programs on top of [WebAssembly types](types.md) making use of the provided [environment](environment.md), but it is not as easy as just compiling existing code with another compiler.

