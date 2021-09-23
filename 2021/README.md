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

# [TypeScript] Variadic Tuple Types

```ts
function tail<T extends any[]>(arr: readonly [any, ...T]) {
    const [_ignored, ...rest] = arr;
    return rest;
}

const myTuple = [1, 2, 3, 4] as const;
const myArray = ["hello", "world"];

// type [2, 3, 4]
const r1 = tail(myTuple);

// type [2, 3, 4, ...string[]]
const r2 = tail([...myTuple, ...myArray] as const);
```

```ts
type Arr = readonly any[];

function concat<T extends Arr, U extends Arr>(arr1: T, arr2: U): [...T, ...U] {
    return [...arr1, ...arr2];
}
```

# [TypeScript] Labeled Tuple Elements

```ts
type Range = [start: number, end: number];

type Foo = [first: number, second?: string, ...rest: any[]];
```

# [TypeScript] compound assignment operators

```ts
let values: string[];

// Before
(values ?? (values = [])).push("hello");

// After
(values ??= []).push("hello");
```

```ts
obj.prop ||= foo();

// roughly equivalent to either of the following

obj.prop || (obj.prop = foo());

if (!obj.prop) {
    obj.prop = foo();
}
```

# [TypeScript] unknown on catch Clause Bindings

TypeScript 4.0 now lets you specify the type of catch clause variables as unknown instead. unknown is safer than any because it reminds us that we need to perform some sorts of type-checks before operating on our values.

```ts
try {
    // ...
}
catch (e: unknown) {
    // error!
    // Property 'toUpperCase' does not exist on type 'unknown'.
    console.log(e.toUpperCase());

    if (typeof e === "string") {
        // works!
        // We've narrowed 'e' down to the type 'string'.
        console.log(e.toUpperCase());
    }
}
```

# AbortController

AbortController is a global utility class used to signal cancelation in selected Promise-based APIs, based on the AbortController Web API. It allows you to abort one or more Web requests as and when desired.

When the fetch request is initiated, we pass in the AbortSignal as an option inside the request's options object (see {signal}, below). This associates the signal and controller with the fetch request and allows us to abort it by calling AbortController.abort(), as seen below in the second event listener.

```ts
const controller = new AbortController();
const signal = controller.signal;

const abortBtn = document.querySelector('.abort');

abortBtn.addEventListener('click', () => {
  controller.abort();
});

function fetchVideo() {
  ...
  fetch(url, {signal}).then(function(response) {
    ...
  }).catch(function(e) {
    reports.textContent = 'Download error: ' + e.message;
  })
}
```

> Note: When abort() is called, the fetch() promise rejects with a DOMException named AbortError.

# AggregateError

The AggregateError object represents an error when several errors need to be wrapped in a single error. It is thrown when multiple errors need to be reported by an operation, for example by Promise.any(), when all promises passed to it reject.

```ts
try {
  throw new AggregateError([
    new Error("some error"),
  ], 'Hello');
} catch (e) {
  console.log(e instanceof AggregateError); // true
  console.log(e.message);                   // "Hello"
  console.log(e.name);                      // "AggregateError"
  console.log(e.errors);                    // [ Error: "some error" ]
}
```

# HTML5 Sectioning Elements

It is important to understand that many HTML5 sectioning (e.g. main,nav, aside ...) elements by default define ARIA landmarks. If HTML5 sectioning elements are used without understanding the associated landmark structure, assistive technology users will most likely be confused and less efficient in accessing content and interacting with web pages.

- aside
- footer
- form
- header
- main
- nav
- section

# `<meter>`

The `<meter>` HTML element represents either a scalar value within a known range or a fractional value.

Note: In `<placeholder>` unlike the `<meter>` element, the minimum value is always 0, and the min attribute is not allowed for the `<progress>` element.

```html
<meter id="fuel"
       min="0" max="100"
       low="33" high="66" optimum="80"
       value="50">
    at 50/100
</meter>
```

# Content categories

Every HTML element is a member of one or more content categories — these categories group elements that share common characteristics. This is a loose grouping (it doesn't actually create a relationship among elements of these categories), but they help define and describe the categories' shared behavior and their associated rules, especially when you come upon their intricate details. It's also possible for elements to not be a member of any of these categories.
  
## Flow content

Flow content is a broad category that encompasses most elements that can go inside the <body> element.

## Sectioning content

Elements belonging to the sectioning content model create a section in the current outline that defines the scope of `<header>` elements, `<footer>` elements, and heading content

Elements belonging to this category are `<article>`, `<aside>`, `<nav>`, and `<section>`.

## Heading content

Heading content is a subset of flow content, which defines the title of a section, whether marked by an explicit sectioning content element, or implicitly defined by the heading content itself.
  
Elements belonging to this category are `<h1>`, `<h2>`, `<h3>`, `<h4>`, `<h5>`, `<h6>` and `<hgroup>`.

## [Phrasing content](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories#phrasing_content)

Phrasing content is a subset of flow content that defines the text and the markup it contains

## [Embedded content](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories#embedded_content)

Embedded content is a subset of flow content that imports another resource or inserts content from another mark-up language or namespace into the document.

## [Interactive content](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories#interactive_content)

Interactive content is a subset of flow content that includes elements that are specifically designed for user interaction.

# JWT
  
  In its compact form, JSON Web Tokens consist of three parts separated by dots (.), which are:

- Header
- Payload
- Signature
  
The header typically consists of two parts: the type of the token, which is JWT, and the signing algorithm being used, such as HMAC SHA256 or RSA.
  
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```
  
  Then, this JSON is Base64Url encoded to form the first part of the JWT.

The second part of the token is the payload, which contains the claims. Claims are statements about an entity (typically, the user) and additional data. There are three types of claims: registered, public, and private claims.

- Registered claims: These are a set of predefined claims which are not mandatory but recommended, to provide a set of useful, interoperable claims. Some of them are: iss (issuer), exp (expiration time), sub (subject), aud (audience), and others.
  
- Public claims: These can be defined at will by those using JWTs. But to avoid collisions they should be defined in the IANA JSON Web Token Registry or be defined as a URI that contains a collision resistant namespace.

- Private claims: These are the custom claims created to share information between parties that agree on using them and are neither registered or public claims.

The payload is then Base64Url encoded to form the second part of the JSON Web Token.

> Do note that for signed tokens this information, though protected against tampering, is readable by anyone. Do not put secret information in the payload or header elements of a JWT unless it is encrypted.
  
To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign that.
For example if you want to use the HMAC SHA256 algorithm, the signature will be created in the following way:
`HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)`

The signature is used to verify the message wasn't changed along the way, and, in the case of tokens signed with a private key, it can also verify that the sender of the JWT is who it says it is.

The output is three Base64-URL strings separated by dots that can be easily passed in HTML and HTTP environments
  
HMAC - Hash-based message authentication code
