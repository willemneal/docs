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
Previous versions of the compiler \(pre 0.7\) did not include any runtime functionality by default but instead required `import`ing an allocator and potentially an experimental tracing collector. This is not supported anymore by recent versions of the compiler.
{% endhint %}

## Future options

The reason for going with our own runtime is that [WebAssembly GC](https://github.com/WebAssembly/gc) is still in the works without any ETA on it. Yet, the absence of runtime functionality was blocking our progress, so we decided to roll our own for the time being. As soon as WebAssembly GC lands, it is very likely that we are going to reconsider alternatives to shipping a runtime, yet most likely still keeping our own around for non-web scenarios that might not implement the GC spec anytime soon.

## Implementation

The memory manager used by AssemblyScript is a variant of [TLSF](http://www.gii.upv.es/tlsf/) \(Two-Level Segregate Fit memory allocator\) and it is accompanied by PureRC \([A Pure Reference Counting Garbage Collector](https://researcher.watson.ibm.com/researcher/files/us-bacon/Bacon03Pure.pdf) devised by David F. Bacon et al.\) with [slight modifications of assumptions](https://github.com/dcodeIO/purerc) to avoid unnecessary work. Essentially, TLSF is responsible for partitioning the memory into chunks that can be used by the various objects, while PureRC keeps track of their lifetimes.

The concept is simple: If a reference to an object is established, its reference count is increased by 1 \(retained\), and when a reference is deleted, its reference count is decreased by 1 \(released\). If the reference count of an object reaches 0, it is collected and its memory returned to TLSF for reuse.

## Performance considerations

Both the TLSF memory manager and the concept of reference counting are a good fit for _predictable performance_ scenarios. Remember: WebAssembly is about exactly that. It is not necessarily the fastest choice in every possible scenario, though.

It is also likely that our implementations are not as optimized yet as ultimately possible. If you are smarter than us, please let us know of your thoughts and findings.

## Internals

Unlike other reference counting implementions, allocating an object starts with a reference count of 0. This is useful in standard library code where memory is first allocated and then assigned to a variable, establishing the first reference.

The compiler does some extra work to evaluate whether an object can potentially become part of a reference cycle. Any object that cannot directly or indirectly reference another object of its kind is considered _inherently acyclic_, so it does not have to be added to the cycle buffer for further evaluation by deferred garbage collection.

Due to the lack of random access to WebAssembly's execution stack \(remember: AssemblyScript does not have a stack on its own\), values returned from a function become pre-retained for the caller, expecting that it will release the reference at the end, or give the reference to its own caller. This also leads to situations where sometimes a reference must be retained on assignment, and sometimes it must not. It also leads to situations where two branches must be unified, for example if there is a pre-retained value in one arm and one that is not in the other. The compiler is taking care of these situations, but it also requires special care when dealing with the reference counting internals within the standard library.

The compiler also utilizes a concept of so called autorelease locals. Essentially, if a reference enters a block of code from somewhere else, is pre-retained but not immediately retained, such a local is added to the current scope to postpone the necessary release until the scope is exited.

One thing not particularly ideal at this point is that each function is expected to retain the reference-type arguments it is given, and release them again at the end. This is necessary because the compiler does just a single pass and doesn't know whether a variable becomes reassigned beforehand \(no SSA or similar, that's left to Binaryen\). It does already skip this for certain variables like `this` that cannot be reassigned, but there are definitely more optimization opportunities along the way. Due to AssemblyScript essentially being a compiler on top of another compiler \(here: Binaryen\), it is somewhat unclear however where to perform such optimizations. For instance, reimplementing half of Binaryen doesn't seem like a good option.

If you are interested in the inner workings, the internal APIs are explained in [the runtime's README file](https://github.com/AssemblyScript/assemblyscript/tree/master/std/assembly/rt) and of course in its sources - feel free to take a look!

