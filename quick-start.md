---
description: Both the easiest and the fastest way to compile your first program.
---

# Quick start

To get started with AssemblyScript, switch to a new directory and point [npm](https://www.npmjs.com) at the GitHub repository \(for now\)

```text
$> npm install --save-dev AssemblyScript/assemblyscript
```

followed by scaffolding a new project, for example in the current directory:

```text
npx asinit .
```

The `asinit` command automatically creates the recommended directory structure and configuration files, including:

* The `assembly/` directory containing the sources being compiled to WebAssembly, with a `tsconfig.json` telling your editor about AssemblyScript's standard library along an exemplary `index.ts` .
* The `build/` directory where compiled WebAssembly binaries, source maps, definition files etc. become placed.
* A `package.json` with AssemblyScript as a development dependency and build tasks to compile both an untouched \(no optimizations yet\) and an optimized version of your program.

Afterwards, simply edit the sources an `assembly/`, maybe tweak the build steps in `package.json` to fit your needs, and run `npm run asbuild` to compile your program to WebAssembly.





