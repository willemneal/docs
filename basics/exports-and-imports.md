---
description: Setting up the API to talk to your WebAssembly.
---

# Exports and imports

## Exports

Exports work very much like in TypeScript, with the notable difference that exports from an entry file also become the exports of the resulting WebAssembly module.

### Functions

{% code-tabs %}
{% code-tabs-item title="index.ts" %}
```typescript
export function add(a: i32, b: i32): i32 {
  return a + b;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Globals

{% code-tabs %}
{% code-tabs-item title="index.ts" %}
```typescript
export const foo = 1;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Exporting mutable globals, if supported by the host, can be enabled through `--enable mutable-global` and will otherwise result in a warning with the export being skipped. If mutable globals are not supported by the host, it is currently recommended to manually export a getter and a setter function instead.

### Classes

If an entire class \(possibly part of a namespace\) is exported from an entry file, its visible members will become distinct module exports using a JSDoc-like naming scheme that [the loader](loader.md) understands to make a nice object structure of. For example

```typescript
export namespace foo {
  export class Bar {
    a: i32 = 1;
    getA(): i32 { return this.a; }
  }
}
```

will yield the function exports

* foo.Bar\#**constructor**
* foo.Bar\#**get:a**
* foo.Bar\#**set:a**
* foo.Bar\#**getA**

which can be used externally just like:

```javascript
var thisBar = myModule["foo.Bar#constructor"]();
myModule["foo.Bar#set:a"](thisBar, 2);
console.log(myModule["foo.Bar#getA"](thisBar));
```

For instance members, the `this` argument must be provided as an additional first argument. No argument or `0` as the first argument to a constructor indicates that the constructor is expected to allocate on its own - this is a mechanism in place to also be able to inherit memory from a subclass's allocation. One usually doesn't have to deal with this manually, though, since the loader will already take care of it. See the [loader documentation](loader.md) for more information.

## Imports

With [WebAssembly ES Module Integration](https://github.com/WebAssembly/esm-integration) still in the pipeline, imports utilize the ambient context currently. For example

{% code-tabs %}
{% code-tabs-item title="env.ts" %}
```typescript
export declare function doSomething(foo: i32): void;
```
{% endcode-tabs-item %}
{% endcode-tabs %}

creates an import of a function named `doSomething` within the `env` module, because that's the name of the file it lives is. It is also possible to use namespaces:

{% code-tabs %}
{% code-tabs-item title="foo.ts" %}
```typescript
declare namespace console {
  export function logi(i: i32): void;
  export function logf(f: f64): void;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

This will import the functions `console.logi` and `console.logf` from the `foo` module. Bonus: Don't forget `export`ing namespace members if you'd like to call them from outside the namespace.

### Custom naming

Where automatic naming is not sufficient, the `@external` decorator can be used to give an element another external name:

{% code-tabs %}
{% code-tabs-item title="bar.ts" %}
```typescript
@external("doSomethingElse")
export declare function doSomething(foo: i32): void;
// imports bar.doSomethingElse as doSomething

@external("foo", "baz")
export declare function doSomething(foo: i32): void;
// imports foo.baz as doSomething
```
{% endcode-tabs-item %}
{% endcode-tabs %}

