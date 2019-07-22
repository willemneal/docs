---
description: A collection of examples that we and others have created over time.
---

# Examples

## Wasm By Example

> [Wasm By Example](https://wasmbyexample.dev/all-examples-list.html) is a concise, hands-on introduction to WebAssembly using code snippets and annotated example programs. If you "learn best by doing", or just need a good starting point for a concept, this is the place for you.

## Example programs

The repository contains a set of simple programs making use of AssemblyScript's low-level capabilities.

### [Conway's Game Of Life](https://github.com/AssemblyScript/assemblyscript/tree/master/examples/game-of-life) \([demo](https://assemblyscript.github.io/assemblyscript/examples/game-of-life/)\)

An implementation of the game of life with slight modifications. Updates an image buffer in memory, that is then presented on a canvas.

### [Mandelbrot Set](https://github.com/AssemblyScript/assemblyscript/tree/master/examples/mandelbrot) \([demo](https://assemblyscript.github.io/assemblyscript/examples/mandelbrot/)\)

Computes 2048 offsets of a color gradient in memory, line by line, and presents the set using the gradient's actual colors, as computed on the JavaScript side, on a canvas.

## Example libraries

The following libraries make use of WebAssembly features specifically in order to complement JavaScript.

### [i64](https://github.com/AssemblyScript/assemblyscript/tree/master/lib/i64)

Exposes WebAssembly's native i64 operations to JavaScript by means of splitting values into its low and high 32 bits and performing the respective operations on the full 64 bits.

### [libm](https://github.com/AssemblyScript/assemblyscript/tree/master/lib/libm)

AssemblyScript's native math routines as a library. Both double \(`f64`\) and single precision \(`f32)` variants that work very much like JavaScript's `Math`.

### [parse](https://github.com/AssemblyScript/assemblyscript/tree/master/lib/parse)

A WebAssembly binary parser in WebAssembly itself, evaluating a WebAssembly binary in memory. Useful to quickly evaluate the sections contained in a `.wasm`  file.

## Additional resources

For a list of more sophisticated open source projects using AssemblyScript, see:

{% page-ref page="community/built-with-assemblyscript.md" %}



