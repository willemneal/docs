---
description: Special features that exist for the sole reason of why not.
---

# Peculiarities

## Annotations

Decorators work more like actual compiler annotations in AssemblyScript.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Annotation</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">@inline</td>
      <td style="text-align:left">Requests inlining of a constant or function.</td>
    </tr>
    <tr>
      <td style="text-align:left">@lazy</td>
      <td style="text-align:left">Requests lazy compilation of a variable. Useful to avoid unnecessary globals.</td>
    </tr>
    <tr>
      <td style="text-align:left">@global</td>
      <td style="text-align:left">Registers an element to be part of the global scope.</td>
    </tr>
    <tr>
      <td style="text-align:left">@external</td>
      <td style="text-align:left">Changes the external name of an imported element. <code>@external(module, name)</code> changes
        both the module and element name, <code>@external(name)</code> changes the
        element name only.</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>@operator,</p>
        <p>@operator.binary</p>
      </td>
      <td style="text-align:left">Annotates a method as a binary operator overload. See below.</td>
    </tr>
    <tr>
      <td style="text-align:left">@operator.prefix</td>
      <td style="text-align:left">Annotates a method as a unary prefix operator overload. See below.</td>
    </tr>
    <tr>
      <td style="text-align:left">@operator.postfix</td>
      <td style="text-align:left">Annotates a method as a unary postfix operator overload. See below.</td>
    </tr>
  </tbody>
</table>## Operator overloads

Operator overloads can only be used on class methods. The respective argument types must match the class type.

### Binary operations

```typescript
@operator(OP)
static __op(left: T, right :T): T { ... }

@operator(OP)
__op(right: T): T  { ... }
```

| OP | Description |
| :--- | :--- |
| `"[]"` | Checked indexed get |
| `"[]="` | Checked indexed set |
| `"{}"` | Unchecked indexed get |
| `"{}="` | Unchecked indexed set |
| `"=="` | Equality |
| `"!="` | Inequality |
| `">"` | Greater than |
| `">="` | Greater than or equal |
| `"<"` | Less than |
| `"<="` | Less than or equal |
| `">>"` | Arithmetic right shift |
| `">>>"` | Logical right shift |
| `"<<"` | Left shift |
| `"&"` | Bitwise AND |
| `"|"` | Bitwise OR |
| `"^"` | Bitwise XOR |
| `"+"` | Addition |
| `"-"` | Subtraction |
| `"*"` | Multiplication |
| `"/"` | Division |
| `"**"` | Exponentiation |
| `"%"` | Remainder |

The `===` operation cannot be overloaded \(is identity equality\).

### Unary prefix operations

```typescript
@operator.prefix(OP)
static __op(self: T): T { ... }

@operator.prefix(OP)
__op(): T { ... }
```

| OP | Description |
| :--- | :--- |
| `"!"` | Logical NOT |
| `"~"` | Bitwise NOT |
| `"+"` | Unary plus |
| `"-"` | Unary negation |
| `"++"` | Prefix increment |
| `"--"` | Prefix decrement |

### Unary postfix operations

```typescript
@operator.postfix(OP)
static __op(self: T): T { ... }

@operator.postfix(OP)
__op(): T { ... }
```

| OP | Description |
| :--- | :--- |
| `"++"` | Postfix increment |
| `"--"` | Postfix decrement |

{% hint style="info" %}
Overloaded postfix operations do not preserve the original value automatically.
{% endhint %}

## Tree-shaking

The compiler only compiles elements reachable from the entry file and ignores everything that remains unused. This is very similar to a JavaScript VM's behavior when running a JavaScript file, but unlike in TypeScript it also affects checking of types. Helps to reduce compilation times and binary sizes significantly.

## Transforms

The compiler frontend \(asc\) provides hooking into the compilation process by means of transforms. Specifying `--transform myTransform` on the command line will load the node module pointed to by `myTransform` and the compiler will call the following hooks during the compilation process:

* **afterParse**\(parser: `Parser`\): `void` Called with the parsing result of all relevant files once parsing is complete. Useful to modify the AST before it becomes compiled, for example by looking for custom decorators and injecting actual logic.

The set of hooks is intentionally minimal at this point. If you need something special, please let us know about your use case.

