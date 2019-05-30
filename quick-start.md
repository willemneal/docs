---
description: Paving the way to what might be your first WebAssembly module.
---

# Quick start

To get started with AssemblyScript, switch to a new directory and point [npm](https://www.npmjs.com) at the GitHub repository \(for now\)

```text
$> npm install --save-dev AssemblyScript/assemblyscript
```

followed by scaffolding a new project, for example in the current directory:

```text
$> npx asinit .
```

The `asinit` command automatically creates the recommended directory structure and configuration files, including:

* The `assembly/` directory containing the sources being compiled to WebAssembly, with a `tsconfig.json` telling your editor about AssemblyScript's standard library along an exemplary `index.ts` .
* The `build/` directory where compiled WebAssembly binaries, source maps, definition files etc. become placed.
* A `package.json` with AssemblyScript as a development dependency and build tasks to compile both an untouched \(as emitted by the AssemblyScript compiler\) and an optimized \(using Binaryen\) version of your program in both binary and text format.

Afterwards, edit the sources in `assembly/`, maybe tweak the build steps in `package.json` to fit your needs, and run `npm run asbuild` to compile your program to WebAssembly.

Using the exemplary `index.js` in the root directory of your package you'll then be able to `require`the WebAssembly module just like any other node module, with the notable difference that the only values your module's exports understand being integers and floats. That's of course not the end of the story. In fact, we're just getting started.



