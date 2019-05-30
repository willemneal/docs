---
description: throw new Error("not implemented")
---

# Limitations

Not everything that can be implemented has been implemented yet and there are features that are not yet feasible to implement due to the relevant [post-MVP WebAssembly features](https://webassembly.org/docs/future-features/) still being on their way through specification.

One of the most limiting factors at this point is the lack of closures. Take for example

```typescript
let sum = 0;
myArray.forEach(value => {
  sum += value; // can't find "sum"
});
```

Quite a bummer, right? Of course there are ways to do it differently. One can, for example, make `sum` a global variable that can be accessed everywhere, or write this snippet differently:

```typescript
let sum = 0;
for (let i = 0, k = myArray.length; i < k; ++i) {
  sum += myArray[i]; // works
}
```

This also means that we cannot yet have shiny things like iterators, because these are basically closures.

Another major bummer is that object oriented programming is still missing some of the building blocks to support interfaces \(here: virtual members in general\). One can have classes possibly extending others classes, though, just to hit another limitation:

```typescript
class A {
  foo(): void {}
}

class B extends A {
  foo(): void {}
}

var a: A = new B();
a.foo(); // calls A#foo, not the overloaded B#foo
```

Again, there are ways to write code that does the right thing.

```typescript
if (a instanceof B) {
  let b = a as B;
  b.foo();
}
```

Luckily, the OOP limitations are something on our end, not necessarily WebAssembly's, and we are working on it as we speak.

