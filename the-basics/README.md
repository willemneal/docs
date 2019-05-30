---
description: 'There is something appealing to it, isn''t it?'
---

# The Basics

Unlike TypeScript, which targets a JavaScript environment with all of its dynamic features, AssemblyScript targets WebAssembly, intentionally avoiding the dynamicness of JavaScript where it cannot be compiled ahead of time _efficiently_.

WebAssembly's [type system](types.md) differs from JavaScript's in that it has more specific integer \(`i32` and `i64`\) and floating point \(`f32` and `f64`\) types, yet it has no higher level concept of strings or classes on its own. However, as usually in computer science, higher level functionality can be implemented with just these. One can think of AssemblyScript as if C and JavaScript had a somewhat special child that tries to be more like JavaScript.

At this point in time, it must also be noted that WebAssembly runs in a sandbox with no immediate access to the DOM or other JavaScript APIs. Basic parts of the [environment](environment.md) can be implemented on the standard library side, while other parts need to be imported from the host, sometimes requiring a bit of JavaScript glue code to translate between objects and numbers.

Combined, this makes it unlikely that an existing TypeScript program can be compiled to WebAssembly without modifications, yet not so unlikely that an already reasonably strict TypeScript program can be made compatible with the AssemblyScript compiler.

But what does this mean _exactly_? Well, let there be...

## Differences

For a better understanding of what to expect, let's start with a collection of obviously "not so strictly typed" code snippets, and how to fix them.

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

AssemblyScript uses `===` for identity comparisons \(means: the exact same object\). Idea is that its special JavaScript semantics for strings \(same type and value\) become irrelevant in a strict type context. Some like this better, some hate it for portability reasons. Feel free to discuss!

```typescript
var s1 = "1";
var s2 = "1".charAt(0);
s1 === s1 // true
s1 === s2 // false
s2 === s2 // true
s1 === 1 // compile error
s1 == s2 // true
```

### Conclusion

Keeping current [technical limitations](limitations.md) in mind as well, this means it is already possible to create working programs on top of [WebAssembly types](types.md) making use of the provided [environment](environment.md), but it is not as easy as just compiling existing code with another compiler.

