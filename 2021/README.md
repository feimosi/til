# Equality comparisons, SameValueZero

There are four equality algorithms in ES2015:
- Abstract Equality Comparison (`==`)
- Strict Equality Comparison (`===`): used by `Array.prototype.indexOf`, `Array.prototype.lastIndexOf`, and case-matching
- SameValueZero: used by `%TypedArray%` and `ArrayBuffer` constructors, as well as `Map` and `Set` operations, and also `String.prototype.includes` and `Array.prototype.includes` since ES2016
- SameValue: used in all other places

JavaScript provides three different value-comparison operations:
- `===` - Strict Equality Comparison ("strict equality", "identity", "triple equals")
- `==` - Abstract Equality Comparison ("loose equality", "double equals")
- `Object.is` provides SameValue (new in ES2015).

# [TypeScript] Enum reverse mappings

In addition to creating an object with property names for members, numeric enums members also get a reverse mapping from enum values to enum names.  
An enum is compiled into an object that stores both forward (name -> value) and reverse (value -> name) mappings. 


```ts
enum Enum {
  A,
}

Enum[A] // 0
Enum[Enum.A] // A
```

# [TypeScript] Const Enums

In most cases, enums are a perfectly valid solution. However sometimes requirements are tighter. To avoid paying the cost of extra generated code and additional indirection when accessing enum values, it’s possible to use const enums. Const enums can only use constant enum expressions and unlike regular enums they are completely removed during compilation.

```ts
const enum Direction {
  Up,
  Down,
  Left,
  Right,
}

let directions = [
  Direction.Up,
  Direction.Down,
  Direction.Left,
  Direction.Right,
];
```

in generated code will become

```js
"use strict";
let directions = [
    0 /* Up */,
    1 /* Down */,
    2 /* Left */,
    3 /* Right */,
];
```
