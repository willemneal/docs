---
description: An explanation of the basic AssemblyScript concepts.
---

# The Basics

Unlike TypeScript, which targets a JavaScript environment and can fall back to all its dynamic features, AssemblyScript targets WebAssembly and does not support falling back to dynamic features of JavaScript that cannot be compiled ahead of time _efficiently_. This makes it unlikely that an existing TypeScript program can be compiled to WebAssembly without modifications, but it also is not unlikely that an already reasonably strict TypeScript program can be made compatible with the AssemblyScript compiler.

With WebAssembly also comes a [new set of types](types.md) and a [WebAssembly-specific environment](environment.md) that differ from what one is already used to in a browser or under node.js.

