---
description: Imagine brains without the logic.
---

# Memory

Similar to other languages that use linear memory, all data in AssemblyScript becomes stored at a specific memory offset so other parts of the program can read and modify it.

The [WebAssembly.Memory](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly/Memory) instance used by your program can be imported from the host by using the `--importMemory` flag on the command line. The module will then expect an import named `memory`. Likewise, a module will always export its memory as `memory`.

Internally, there are two regions of memory the compiler is aware of:

### Static memory

Memory starts with static data, like strings and arrays the compiler encountered while translating the program. Unlike other languages, there is no concept of a stack in AssemblyScript and it instead relies on WebAssembly's execution stack exclusively.

### Dynamic memory

Dynamic memory, commonly known as the heap, is managed by the [AssemblyScript runtime](runtime.md), at runtime. When a chunk of memory is requested by the program, the runtime's memory manager reserves a suitable region and returns a pointer to the start of the chunk to the program. The lifetime of objects allocated from the heap is then tracked by the runtime's garbage collector, and once it is not needed anymore, the object's chunk of memory is returned to the memory manager for reuse.

### Internals

When writing high level AssemblyScript, the above is pretty much everything one needs to know. But there is a low level perspective of course.

Objects in AssemblyScript have a common hidden header used by the runtime to keep track of them. The header includes information about the block used by the memory manager, state information used by the garbage collector, a unique id per concrete class and the data's actual size. The length of a `String` \(id = 1\) is computed from the data's size for example. The header is "hidden" in that the reference to an object points right behind it, at the first byte of the object's actual data.

```text
╒════════════════ Common block layout (32-bit) ═════════════════╕
   3                   2                   1
 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0  bits
├─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┤
│                           MM info                             │ -16
├───────────────────────────────────────────────────────────────┤
│                           GC info                             │ -12
├───────────────────────────────────────────────────────────────┤
│                          runtime id                           │ -8
├───────────────────────────────────────────────────────────────┤
│                         runtime size                          │ -4
╞═══════════════════════════════════════════════════════════════╡
│                              ...                              │ ref
```

The most basic objects in AssemblyScript are `ArrayBuffer` and `String` as these do not have any fields but are just data.

```text
╒══════════════════ ArrayBuffer layout (32-bit) ════════════════╕
   3                   2                   1
 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0  bits
├─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┤
│       [0]     │      [1]      │      [2]      │      [3]      │ N=.byteLength
├ ─ ─ ─ ─ ─ ─ ─ ┼ ─ ─ ─ ─ ─ ─ ─ ┼ ─ ─ ─ ─ ─ ─ ─ ┼ ─ ─ ─ ─ ─ ─ ─ ┤ 8-bit bytes
│       [4]     │      [5]      │      [6]      │      [7] ...  │
```

```text
╒════════════════════ String layout (32-bit) ═══════════════════╕
   3                   2                   1
 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0 9 8 7 6 5 4 3 2 1 0  bits
├─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┤
│         .charCodeAt(0)        │        .charCodeAt(1)         │ N=.length 16-bit
├ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┼ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ┤ character codes
│         .charCodeAt(2)        │        .charCodeAt(3) ...     │
```

Unlike other languages, strings use UTF16 encoding in AssemblyScript to match common JavaScript APIs in an attempt to avoid reencoding on every JS-API call. While this helps to reduce the overhead when talking to the host, it introduces some overhead on lower levels, like when integrating AssemblyScript into a C environment.

