---
description: 'How to import and export memory, and all the details on its layout.'
---

# Memory

Similar to other languages that use linear memory, all data in AssemblyScript becomes stored at a specific memory offset so other parts of the program can read and modify it.

The [WebAssembly.Memory](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly/Memory) instance used by your program can be imported from the host by using the `--importMemory` flag on the command line. The module will then expect an import named `memory`. Likewise, a module will always export its memory as `memory`.

Internally, there are two regions of memory the compiler is aware of:

### Static memory

Memory starts with static data, like strings and arrays \(of constant values\) the compiler encountered while translating the program. Unlike in other languages, there is no concept of a stack in AssemblyScript and it instead relies on WebAssembly's execution stack exclusively.

A custom region of memory can be reserved using the `--memoryBase` option. For example, if one needs an image buffer of exactly N bytes, instead of allocating it one could reserve that space, telling the compiler to place its own static data afterwards.

### Dynamic memory

Dynamic memory, commonly known as the heap, is managed by the [AssemblyScript runtime](runtime.md), at runtime. When a chunk of memory is requested by the program, the runtime's memory manager reserves a suitable region and returns a pointer to it to the program. The lifetime of objects allocated from the heap is then tracked by the runtime's garbage collector, and once it is not needed anymore, the object's chunk of memory is returned to the memory manager for reuse.

### Internals

When writing high level AssemblyScript, the above is pretty much everything one needs to know. But there is a low level perspective of course.

When implementing low-level code, the end of static memory respectively the start of dynamic memory can be obtained by reading the `__heap_base` global. Memory managers for example do this to decide where to place their bookkeeping structures before partitioning the heap.

Objects in AssemblyScript have a common hidden header used by the runtime to keep track of them. The header includes information about the block used by the memory manager, state information used by the garbage collector, a unique id per concrete class and the data's actual size. The length of a `String` \(id = 1\) is computed from that size for example. The header is "hidden" in that the reference to an object points right after it, at the first byte of the object's actual data.

#### Common header layout

| Name | Offset | Type | Description |
| :--- | :--- | :--- | :--- |
| \#mmInfo | -16 | usize | General block information |
| \#gcInfo | -12 | usize | Reference count etc. |
| \#rtId | -8 | u32 | Unique id of the concrete class |
| \#rtSize | -4 | u32 | Size of the data following the header |

The most basic objects using the common header are `ArrayBuffer` and `String`. These do not have any fields but are just data:

#### ArrayBuffer layout

| Name | Offset | Type | Description |
| :--- | :--- | :--- | :--- |
|  | -16 | See above | Common header |
| \[0\] | 0 | 8-bit untyped | The first byte |
| \[1\] | 1 | 8-bit untyped | The second byte |
| ... |  |  |  |
| \[N\] | N | 8-bit untyped | The N-th byte |

#### String layout

| Name | Offset | Type | Description |
| :--- | :--- | :--- | :--- |
|  | -16 | See above | Common header |
| \[0\] | 0 | u16 | The first character |
| \[1\] | 2 | u16 | The second character |
| ... |  |  |  |
| \[N\] | N &lt;&lt; 1 | u16 | The N-th character |

Unlike other languages, strings in AssemblyScript use UTF16 encoding to match common JavaScript APIs in an attempt to avoid re-encoding on every JS-API call. While this helps to reduce the overhead when talking to the host, it can of course introduce some overhead when integrating AssemblyScript into a C environment.

Collections like `Array`, `Map` and `Set` use one or multiple `ArrayBuffer`s to store their data, but the backing buffer is exposed by the typed array views only because other collections can grow automatically, which would otherwise lead to no longer valid references sticking around.

#### ArrayBufferView layout

This is not an actual class but a helper to simplify the implementation of the typed array views. Each typed array view like `Uint8Array` is like a subclass of `ArrayBufferView` with a static value type attached.

| Name | Offset | Type | Description |
| :--- | :--- | :--- | :--- |
|  | -16 | See above | Common header |
| \#data | 0 | ArrayBuffer | Backing buffer reference |
| \#dataStart | 4 | usize | Start of the data within .data |
| \#dataLength | 8 | u32 | Length of the data from .dataStart |

#### Array layout

| Name | Offset | Type | Description |
| :--- | :--- | :--- | :--- |
|  | -16 | See above | Common header |
|  | 0 | See above | ArrayBufferView |
| .length | 12 | i32 | Mutable length of the data the user is interested in |

The layouts of `Set` and `Map` are a bit more sophisticated and therefore not mentioned here, but feel free to take a look at their sources.

