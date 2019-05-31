---
description: 'Because nobody enjoys being bullied by malloc, free and friends.'
---

# Runtime

AssemblyScript's runtime takes care of all the ins and outs of memory management and garbage collection, yet the compiler lets a developer chose the ideal runtime variant for their use case.

## Runtime variants

| Variant | Description |
| :--- | :--- |
| full | A proper memory manager and reference-counting based garbage collector, with runtime interfaces being exported to the host for being able to create managed objects externally. |
| half | The same as `full` but without any exports, i.e. where creating objects externally is not required. This allows the optimizer to eliminate parts of the runtime that are not needed. |
| stub | A minimalist arena memory manager without any means of freeing up memory again, but the same external interface as `full`. Useful for very short-lived programs or programs with hardly any memory footprint, while keeping the option to switch to `full` without any further changes. No garbage collection. |
| none | The same as `stub` but without any exports, for the same reasons as explained in `half`. Essentially evaporates entirely after optimizations. |

The default runtime included in a program is the `full` runtime, but deciding for another variant using the `--runtime` option can help to reduce binary size significantly. The `full` runtime adds about 2KB of additional optimized code to your module, which is not that much considering what it brings to the table, but might still be overkill for simple programs.

{% hint style="info" %}
Previous versions of the compiler \(pre 0.7\) did not include any runtime functionality by default but instead required `import`ing an allocator and potentially a `collector`. This is not supported anymore by recent versions of the compiler.
{% endhint %}

## Implementation

The memory manager used by AssemblyScript is a variant of [TLSF](http://www.gii.upv.es/tlsf/) \(two-level segregate fit memory allocator\) and it is accompanied by PureRC \([a pure reference counting garbage collector](https://researcher.watson.ibm.com/researcher/files/us-bacon/Bacon03Pure.pdf) devised by David F. Bacon et al.\) with [a slight modification of assumptions](https://github.com/dcodeIO/purerc). Essentially, TLSF is responsible for partitioning the memory into chunks that can be used by the various objects, while PureRC keeps track of their lifetimes. If a reference to an object is established, its reference count is increased by 1 \(retained\), and when a reference is deleted, its reference count is decreased by 1 \(released\). If the reference count of an object reaches 0, it is collected and its memory returned to TLSF for reuse.

## Future options

The reason for going with our own runtime is that [WebAssembly GC](https://github.com/WebAssembly/gc) is still in the works without any ETA on it. Yet, the absence of runtime functionality was blocking our progress, so we decided to roll our own for the time being. As soon as WebAssembly GC lands, it is very likely that we are going to reconsider alternatives to shipping a runtime, yet most likely still keeping our own around for non-web scenarios that might not implement the GC spec anytime soon.

## Performance considerations

Both the TLSF memory managed and reference counting in general are especially well-suited for _predictable performance_ scenarios. This makes them a great fit for WebAssembly, which is about exactly that, but not necessarily the fastest choice in every possible scenario.

It is also likely that our implementations are not as optimized yet as ultimately possible. If you are smarter than us, please let us know of your thoughts and findings.

## Internals

The full internals are explained in [the runtime's README file](https://github.com/AssemblyScript/assemblyscript/tree/master/std/assembly/rt) and of course in its sources - feel free to take a look.

