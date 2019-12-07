---
description: How to talk to AssemblyScript from C or other languages.
---

# Interoperability

## Class layout

Layout of objects in memory resembles that of non-packed C structs, and `@unmanaged` classes \(not tracked by GC\) can be used to describe them:

```cpp
struct Foo {
  uint32_t bar;
}
```

```typescript
@unmanaged class Foo {
  bar: u32
}

var foo = changetype<Foo>(ptrToFoo)
```

More complex structures usually require manual offset calculation, though, mostly for the reason that it is not feasible to implement something more specific into AssemblyScript as it does not use such structures on its own:

```cpp
struct Foo {
  int32_t bar;
  uint32_t baz[10];
}
```

```typescript
@unmanaged class Foo {
  bar: i32
  getBaz(i: i32): u32 {
    return load<u32>(
      changetype<usize>(this) + (<usize>i << alignof<u32>()),
      4 // or use offsetof, sizeof etc. to calculate the base offset incl. alignment
    )
  }
}
```

## Calling convention

Exported functions will properly wrap return values, but functions not visible externally do not guarantee proper wrapping.

```typescript
export function getU8(): u8 {
  return someU8 // will wrap to someU8 & 0xff
}
```

On the other hand, calling any AssemblyScript function does not require wrapping of arguments.

## Garbage collection

All the rules described in the [managed runtime](runtime.md) section apply here, except that unmanaged classes do not become tracked by GC and are merely views on a region of memory.

