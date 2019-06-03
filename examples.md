---
description: A collection of examples that we and others have created over time.
---

# Examples

The repository contains a set of examples, mostly making use of AssemblyScript's low-level capabilities.

## Plain examples

### [Conway's Game Of Life](https://github.com/AssemblyScript/assemblyscript/tree/master/examples/game-of-life) \([demo](https://assemblyscript.github.io/assemblyscript/examples/game-of-life/)\)

An implementation of the game of life with slight modifications. Updates an image buffer in memory, that is then presented on a canvas.

### [Mandelbrot Set](https://github.com/AssemblyScript/assemblyscript/tree/master/examples/mandelbrot) \([demo](https://assemblyscript.github.io/assemblyscript/examples/mandelbrot/)\)

Computes 2048 offsets of a color gradient in memory, line by line, and presents the set using the gradient's actual colors, as computed on the JavaScript side, on a canvas.

## Useful libraries

### [i64](https://github.com/AssemblyScript/assemblyscript/tree/master/lib/i64)

Exposes WebAssembly's native i64 operations to JavaScript by means of splitting values into its low and high 32 bits and performing the respective operations on the full 64 bits.

### [libm](https://github.com/AssemblyScript/assemblyscript/tree/master/lib/libm)

AssemblyScript's native math routines as a library. Both double \(`f64`\) and single precision \(`f32)` variants that work very much like JavaScript's `Math`.

### [parse](https://github.com/AssemblyScript/assemblyscript/tree/master/lib/parse)

A WebAssembly binary parser in WebAssembly itself, evaluating a WebAssembly binary in memory. Useful to quickly evaluate the sections contained in a `.wasm`  file.

## Additional resources

For a list of more sophisticated open source projects using AssemblyScript, see:

{% page-ref page="community/built-with-assemblyscript.md" %}



