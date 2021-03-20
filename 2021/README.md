# Equality comparisons, SameValueZero

There are four equality algorithms in ES2015:
- Abstract Equality Comparison (`==`)
- Strict Equality Comparison (`===`): used by `Array.prototype.indexOf`, `Array.prototype.lastIndexOf`, and case-matching
- SameValueZero: used by `%TypedArray%` and `ArrayBuffer` constructors, as well as `Map` and `Set` operations, and also `String.prototype.includes` and `Array.prototype.includes` since ES2016
- SameValue: used in all other places

JavaScript provides three different value-comparison operations:
- `===` - Strict Equality Comparison ("strict equality", "identity", "triple equals")
- `==` - Abstract Equality Comparison ("loose equality", "double equals")
- `Object.is` provides SameValue (new in ES2015). `+0` doesn't equal `-0` and `NaN` equals `NaN`

➡ https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness

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

# [TypeScript] `unknown` vs `any`

With an unknown type we cannot reassign it to a different type, manipulate it, or pass it to another function that has a specified type:

```ts
const numberData: number = unknownData
// ❌ Type 'unknown' is not assignable
// to type 'number'.

const stringData = unknownData.toString()
// ❌ Object is of type 'unknown'.

const dataKeys = Object.keys(unknownData)
// ❌ Argument of type 'unknown' is not
// assignable to parameter of type 'object'.

const dataBool = unknownData as boolean
// ⚠️ Type assertions workaround typing
// but could result in runtime issues
```

> The reason why we were able to pass data to JSON.stringify() is because the type of the value it accepts is actually any. An unknown value can be passed to an any type.

The any type on the other hand allows us to do anything with a value including access arbitrary properties, even ones that don’t exist. We’re basically reverting back to plain ol’ JavaScript at this point.

```ts
const numberData: number = anyData
// 😨 No error

const stringData = anyData.toString()
// 😨 No error calling `.toString()`

const dataKeys = Object.keys(anyData)
// 😨 No error, let's hope `anyData`
// is an `object`
```

➡ https://www.benmvp.com/blog/when-use-typescript-unknown-versus-any/

# [TypeScript] abstract constructor signatures

TypeScript 4.2 allows you to specify an abstract modifier on constructor signatures.

Adding the abstract modifier to a construct signature signals that you can pass in abstract constructors. It doesn’t stop you from passing in other classes/constructor functions that are “concrete” - it really just signals that there’s no intent to run the constructor directly, so it’s safe to pass in either class type.

```ts
abstract class Shape {
  abstract getArea(): number;
}

interface HasArea {
    getArea(): number;
}

let Ctor: abstract new () => HasArea = Shape;

function makeSubclassWithArea(Ctor: new () => HasArea) {
  return class extends Ctor {
    getArea() {
      return 42
    }
  };
}

let MyShape = makeSubclassWithArea(Shape);
```

➡ https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-2.html#abstract-construct-signatures

# [TypeScript] `--noUncheckedIndexedAccess`

TypeScript 4.1 ships with a new flag called `--noUncheckedIndexedAccess`. Under this new mode, every property access (like `foo.bar`) or indexed access (like `foo["bar"]`) is considered potentially undefined. That means that in our last example, `opts.yadda` will have the type `string | number | undefined` as opposed to just `string | number`. If you need to access that property, you’ll either have to check for its existence first or use a non-null assertion operator (the postfix `!` character).

➡ https://www.typescriptlang.org/docs/handbook/release-notes/typescript-4-1.html#checked-indexed-accesses---nouncheckedindexedaccess

# [TypeScript] Key Remapping in Mapped Types

TypeScript 4.1 allows you to re-map keys in mapped types with a new as clause.
With this new as clause, you can leverage features like template literal types to easily create property names based off of old ones.

```ts
type Getters<T> = {
    [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K]
};

interface Person {
    name: string;
    age: number;
    location: string;
}

type LazyPerson = Getters<Person>;
//   ^ = type LazyPerson = {
//       getName: () => string;
//       getAge: () => number;
//       getLocation: () => string;
//   }
```
and you can even filter out keys by producing never. That means you don’t have to use an extra Omit helper type in some cases.

```ts
// Remove the 'kind' property
type RemoveKindField<T> = {
    [K in keyof T as Exclude<K, "kind">]: T[K]
};

interface Circle {
    kind: "circle";
    radius: number;
}

type KindlessCircle = RemoveKindField<Circle>;
//   ^ = type KindlessCircle = {
//       radius: number;
//   }
```

# [React] Extending HTML components with `React.ComponentProps`

```tsx
interface NewProps {
  variant: 'primary' | 'secondary'
  size: 'default' | 'small' | 'large'
}

type Props = NewProps
  & Omit<React.ComponentProps<"button">, keyof NewProps>

const Button = ({ variant, size, ...buttonProps }: Props) => {
  // do stuff with variant & size
  return <button {...buttonProps} />
}
```
