---
description: How to use the compiler from the command line or as an API.
---

# Compiler

 Similar to TypeScript's `tsc` compiling to JavaScript, AssemblyScript's `asc` compiles to WebAssembly.

## Options

```text
SYNTAX
  asc [entryFile ...] [options]

EXAMPLES
  asc hello.ts
  asc hello.ts -b hello.wasm -t hello.wat
  asc hello1.ts hello2.ts -b -O > hello.wasm

OPTIONS
  --version, -v         Prints just the compiler's version and exits.
  --help, -h            Prints this message and exits.
  --optimize, -O        Optimizes the module. Also has the usual shorthands:

                         -O     Uses defaults. Equivalent to -O3s
                         -O0    Equivalent to --optimizeLevel 0
                         -O1    Equivalent to --optimizeLevel 1
                         -O2    Equivalent to --optimizeLevel 2
                         -O3    Equivalent to --optimizeLevel 3
                         -Oz    Equivalent to -O but with --shrinkLevel 2
                         -O3s   Equivalent to -O3 with --shrinkLevel 1 etc.

  --optimizeLevel       How much to focus on optimizing code. [0-3]
  --shrinkLevel         How much to focus on shrinking code size. [0-2, s=1, z=2]
  --validate, -c        Validates the module using Binaryen. Exits if invalid.
  --baseDir             Specifies the base directory of input and output files.
  --outFile, -o         Specifies the output file. File extension indicates format.
  --binaryFile, -b      Specifies the binary output file (.wasm).
  --textFile, -t        Specifies the text output file (.wat).
  --asmjsFile, -a       Specifies the asm.js output file (.js).
  --idlFile, -i         Specifies the WebIDL output file (.webidl).
  --tsdFile, -d         Specifies the TypeScript definition output file (.d.ts).
  --sourceMap           Enables source map generation. Optionally takes the URL
                        used to reference the source map from the binary file.
  --runtime             Specifies the runtime variant to include in the program.

                         full  Default runtime based on TLSF and reference counting.
                         half  Same as 'full', but not exported to the host.
                         stub  Minimal stub implementation without free/GC support.
                         none  Same as 'stub', but not exported to the host.

  --debug               Enables debug information in emitted binaries.
  --noAssert            Replaces assertions with just their value without trapping.
  --noEmit              Performs compilation as usual but does not emit code.
  --importMemory        Imports the memory instance provided by the embedder.
  --sharedMemory        Declare memory as shared by settings the max shared memory.
  --memoryBase          Sets the start offset of compiler-generated static memory.
  --importTable         Imports the function table instance provided by the embedder.
  --explicitStart       Exports an explicit start function to be called manually.
  --lib                 Adds one or multiple paths to custom library components and
                        uses exports of all top-level files at this path as globals.
  --path                Adds one or multiple paths to package resolution, similar
                        to node_modules. Prefers an 'ascMain' entry in a package's
                        package.json and falls back to an inner 'assembly/' folder.
  --use, -u             Aliases a global object under another name, e.g., to switch
                        the default 'Math' implementation used: --use Math=JSMath
  --trapMode            Sets the trap mode to use.

                         allow  Allow trapping operations. This is the default.
                         clamp  Replace trapping operations with clamping semantics.
                         js     Replace trapping operations with JS semantics.

  --runPasses           Specifies additional Binaryen passes to run after other
                        optimizations, if any. See: Binaryen/src/passes/pass.cpp
  --enable              Enables additional (experimental) WebAssembly features.

                         sign-extension  Enables sign-extension operations
                         mutable-global  Enables mutable global imports and exports
                         bulk-memory     Enables bulk memory operations
                         simd            Enables SIMD types and operations.
                         threads         Enables threading and atomic operations.

  --transform           Specifies the path to a custom transform to 'require'.
  --traceResolution     Enables tracing of package resolution.
  --listFiles           Lists files to be compiled and exits.
  --measure             Prints measuring information on I/O and compile times.
  --printrtti           Prints the module's runtime type information to stderr.
  --noColors            Disables terminal colors.
  -- ...                Specifies node.js options (CLI only). See: node --help
```

## API

The compiler can also be used programmatically. Its API accepts the same options as the CLI but also lets you override stdout and stderr and/or provide a callback:

```javascript
const asc = require("assemblyscript/cli/asc");
asc.main([
  "myModule.ts",
  "--binaryFile", "myModule.wasm",
  "--optimize",
  "--sourceMap",
  "--measure"
], {
  stdout: process.stdout,
  stderr: process.stderr
}, function(err) {
  if (err)
    throw err;
  ...
});
```

Available command line options can also be obtained programmatically:

```javascript
const options = require("assemblyscript/cli/asc.json");
...
```

You can also compile a source string directly, for example in a browser environment:

```javascript
const { binary, text, stdout, stderr } = asc.compileString(`...`, { optimize: 2 });
...
```

