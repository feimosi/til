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
