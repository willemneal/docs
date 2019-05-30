---
description: 'There is something appealing to it, isn''t it?'
---

# The Basics

Unlike TypeScript, which targets a JavaScript environment with all of its dynamic features, AssemblyScript targets WebAssembly, intentionally avoiding falling back to super dynamic features of JavaScript that cannot be compiled ahead of time _efficiently_.

WebAssembly's [type system](types.md) differs from JavaScript's in that it has more specific integer \(`i32` and `i64`\) and floating point \(`f32` and `f64`\) types, yet it has no higher level concept of strings or classes. As usually in computer science, higher level functionality can be implemented with just these. One can think of AssemblyScript as if C and JavaScript had a somewhat special child.

At this point in time, it must also be noted that WebAssembly runs in a sandbox with no immediate access to the DOM or other JavaScript APIs. Basic parts of the [environment](environment.md) can be implemented on the standard library side, while other parts need to be imported from the host, sometimes requiring a bit of JavaScript glue code to translate between objects and numbers.

Combined, this makes it unlikely that an existing TypeScript program can be compiled to WebAssembly without modifications, yet not so unlikely that an already reasonably strict TypeScript program can be made compatible with the AssemblyScript compiler.

But what does this mean _exactly_? Well, let there be...

### Differences

For a better understanding of what to expect, let's start with a collection of obviously "not so strictly typed" code snippets, and how to fix them.

There is no `any` or `undefined` for obvious reasons, and there are no union types, ultimately leading to stricter programming patterns:

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

```typescript
// 😢
function foo(a: i32 | string): void {
}

// 😊
function foo<T>(a: T): void {
}
```

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

Seems doable, doesn't it? 

Keeping current [technical limitations](../the-details/limitations.md) in mind as well, this means it's already possible to create a working program using just [WebAssembly types](types.md) and the provided [environment](environment.md), but it is not as easy as just compiling existing code with another compiler.

