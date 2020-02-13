---
description: 'There is something appealing to it, isn''t it?'
---

# The Basics

Unlike TypeScript, which targets a JavaScript environment with all of its dynamic features, AssemblyScript targets WebAssembly, and intentionally **avoids the dynamicness of JavaScript** where it cannot be compiled ahead of time _efficiently_.

The first thing one is going to notice is that AssemblyScript's [type system](types.md) differs from TypeScript's in that it uses WebAssembly's **more specific integer and floating point types**, with `number` merely an alias of `f64`. These types are enough to implement other numerical types and higher level structures like strings and arrays as provided by the [standard library](environment.md#standard-library), yet the relevant specifications \(like [reference types](https://github.com/WebAssembly/reference-types) and [GC](https://github.com/WebAssembly/gc)\) to describe higher level structures to JavaScript are not yet available.

This means that **objects cannot yet flow in and out of WebAssembly natively**, making it necessary to read/write them from/to memory. To make this process more convenient, [the loader](loader.md) provides the utility necessary to translate between the WebAssembly and the JavaScript world.

It must also be noted that WebAssembly has **no immediate access to the DOM** or other JavaScript APIs currently, sometimes making it necessary to create some custom glue code by means of [exports and imports](exports-and-imports.md) to/from JavaScript.

Combined, this makes it **unlikely that existing TypeScript code can be compiled** to WebAssembly without modifications, yet likely that already **reasonably strict TypeScript code can be made compatible** with the AssemblyScript compiler.

But what does this mean _exactly_? Well, let there be...

## Differences

For a better understanding of what to expect, let's start with a collection of obviously "not so strictly typed" code snippets, and how to fix them.

### Strict typing

There is no `any` or `undefined` for obvious reasons:

{% code title="" %}
```typescript
// 😢
function foo(a?) {
  var b = a + 1
  return b
}

// 😊
function foo(a: i32 = 0): i32 {
  var b = a + 1
  return b
}
```
{% endcode %}

There are no union types yet:

{% code title="" %}
```typescript
// 😢
function foo(a: i32 | string): void {
}

// 😊
function foo<T>(a: T): void {
}
```
{% endcode %}

Objects must be strictly typed as well:

{% code title="" %}
```typescript
// 😢
var a = {}
a.prop = "hello world"

// 😊
var a = new Map<string,string>()
a.set("prop", "hello world")

// 😊
class A {
  constructor(public prop: string) {}
}
var a = new A("hello world")
```
{% endcode %}

### The case of ===

Also, AssemblyScript does some things differently. It uses `===` for identity comparisons \(means: the exact same object\) for example. Idea is that its special JavaScript semantics for strings \(same type and value\) become irrelevant in a strict type context anyway. Some like this better, some hate it for portability reasons.

```typescript
var s1 = "1"
var s2 = "123".substring(0, 1)
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

### And the kitchen sink

The examples above mainly exist so one can get an initial idea, but there is of course more, as explained on the following pages, to consider.

