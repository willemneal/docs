---
description: A collection of answers to questions that appeared over time.
---

# Frequently asked questions

## Is WebAssembly _always_ faster?

No, not always. But it can be. The execution characteristics of ahead-of-time compiled WebAssembly differ from just-in-time compiled JavaScript in that it is _more predictable_ in a way that enables a WebAssembly program to remain on a reasonably fast path over the entire course of execution, while a JavaScript VM tries hard to do all sorts of smart optimizations before and _while_ the code is executing. This implies that a JavaScript VM can make both very smart decisions, especially for well-written code with a clear intent, but might also have to reconsider its strategy to do something more general if its assumptions did not hold. If you are primarily interested in performance, our rule of thumb \(that is: from an AssemblyScript perspective\) is:

| Scenario | Recommendation |
| :--- | :--- |
| Compute-heavy algorithm | Use WebAssembly |
| Mostly interacts with the DOM | Mostly use JavaScript |
| Games | Use WebAssembly for CPU-intensive parts |
| WebGL | Depends how much of it is calling APIs. Probably both. |
| Websites, Blogs, ... | JavaScript is your friend |

Or: WebAssembly is great for computational tasks, stuff with numbers, but still needs some time to become more convenient and efficient _at the same time_ where sharing numbers between the module and the host isn't enough.

Bonus: If you are considering another language than AssemblyScript, pick one that doesn't \(currently\) compile an interpreter to WebAssembly to run your code, because that's neither small nor fast.

## How does AssemblyScript compare/relate to C++/Rust?

First and foremost: Both Emscripten \(C++\) and Rust have very mature tooling to compile to WebAssembly and are made by the smartest people on this field. Also, both can make use of compiler infrastructure that has been created by many individuals and corporations over years. In contrast, AssemblyScript is a relatively young project with limited resources that strives to create a viable alternative from another perspective.

More precisely: AssemblyScript is putting anything web - from APIs to syntax to WebAssembly - first and _then_ glues it all together, while others lift an existing ecosystem to the web. Fortunately, there is Binaryen, a compiler infrastructure and toolchain library for WebAssembly primarily created by the main author of Emscripten, that we can utilize to considerably close the gap, and we are very thankful for that. It's not as optimal for AssemblyScript-generated code as it is for LLVM-generated code in a few cases, but it's already pretty good and continuously becoming better. It's also noteworthy that AssemblyScript is still behind in specific language features, especially when it comes to OOP, or even general compiler design - but we are working on that.

In short: AssemblyScript differs in that it is new and tries another approach. It's not as mature as Emscripten and Rust, but there is something about the idea that is definitely appealing. If you find it appealing as well, AssemblyScript is for you.

