# [TypeScript] Indexed access type

```ts
const sizes = ['xs', 'sm', 'md', 'lg', 'xl']

type Values = {
  [K in typeof sizes[number]]: number
}
// which is equivalent to:
type Values = {
  xs: number
  sm: number
  md: number
  lg: number
  xl: number
}
```

`(typeof sizes)[number]` is an indexed access type, which will get the resulting type from indexing `typeof sizes` with an index of type `number`. A more readable translation may be “create a key for each item in `sizes` then set the key value as type `number`.”

➡️ https://codyarose.com/blog/object-keys-from-array-in-typescript/

# Date constructor

Note: Parsing of date strings with the `Date constructor` (and `Date.parse()`, which works the same way) is strongly discouraged due to browser differences and inconsistencies.

- Support for RFC 2822 format strings is by convention only.
- Support for ISO 8601 formats differs in that date-only strings (e.g. "1970-01-01") are treated as UTC, not local.

```js
new Date('2022-06-06') // parsed as UTC: Mon Jun 06 2022 02:00:00 GMT+0200 (Central European Summer Time)
new Date('2022-06-06T00:00') // parsed as local: Mon Jun 06 2022 00:00:00 GMT+0200 (Central European Summer Time)
```

➡️ https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/Date

# [TypeScript] Variance Annotations for Type Parameters

### Definition

Covariance
```ts
fruit = new Fruit();
mango = new Mango();

fruit = mango;
```

Contravariance
```ts
function getFruitInfo(fruit: Fruit);
function getMangoInfo(mango: Mango);

getMangoInfo = getFruitInfo
```
___
### TypeScript syntax

If we want to make it explicit that `Getter` is covariant on `T`, we can now give it an `out` modifier.
```ts
type Getter<out T> = () => T;
```
And similarly, if we also want to make it explicit that `Setter` is contravariant on `T`, we can give it an `in` modifier.
```ts
type Setter<in T> = (value: T) => void;
```

`out` and `in` are used here because a type parameter’s variance depends on whether it’s used in an output or an input. Instead of thinking about variance, you can just think about if `T` is used in output and input positions.

There are also cases for using both in and out.
```ts
interface State<in out T> {
    get: () => T;
    set: (value: T) => void;
}
```

# ES2022

## `Array.prototype.at()`

The `at()` method takes an integer value and returns the item at that index, allowing for positive and negative integers. Negative integers count back from the last item in the array. `array.at(-1)` for the last item.

## `Object.hasOwn()`

The `Object.hasOwn()` static method returns `true` if the specified object has the indicated property as its own property. If the property is inherited, or does not exist, the method returns `false`.

`Object.hasOwn()` vs `Object.prototype.hasOwnProperty()`
Firstly, it can be used with objects that have reimplemented `hasOwnProperty()`. It can also be used to test objects created using `Object.create(null)`. These do not inherit from `Object.prototype`, and so `hasOwnProperty()` is inaccessible.

## `Error` `cause`

`new Error(message, { cause })`

`cause` is a property indicating the specific cause of the error. When catching and re-throwing an error with a more-specific or useful error message, this property can be used to pass the original error.

```js
try {
  frameworkThatCanThrow();
} catch (erroe) {
  throw new Error('New error message', { cause: erroe });
}
```

## RegExp match indices

Adding a flag `/d` to a regular expression produces match object that records the start and end of each group capture (inside `indices` property).

```js
/(a+)(b+)/d.exec('aaaabb');
// ['aaaabb', 'aaaa', 'bb', index: 0, input: 'aaaabb', groups: undefined, indices: Array(3)]
// indices: [[0, 6], [0, 4], [4, 6]]
```

# Filtering arrays with TypeScript type guards

```ts
const firstSquare = shapes.find((shape) => shape.type === "SQUARE");
console.log(firstSquare?.size); // firstSquare has type Shape
//                       ^^^^
// Property 'size' does not exist on type 'Square | Rectangle | Circle'.
//  Property 'size' does not exist on type 'Rectangle'.(2339)
```
Type guards:
```ts
const isSquare = (shape: Shape): shape is Square => shape.type === "SQUARE";
```

```ts
const firstSquare = shapes.find(isSquare);
console.log(firstSquare?.size); // firstSquare has type Square
```

Filtering:
```ts
const onlyCircles = shapes.filter(isCircle);
onlyCircles.forEach((circle) => console.log(circle.radius));
```

Combining
```
const firstSquare = shapes.filter(isSquare).find((shape) => shape.size === 1);
```

➡️ https://www.skovy.dev/blog/typescript-filter-array-with-type-guard

# Get notified in JavaScript when a media query changes

You can watch for changes on media queries with the following JavaScript code:
```js
window.matchMedia('(max-height: 150px)')
    .addListener(console.log)
```
As a response, you get a MediaQueryListEvent which, in addition to including the original media query string, contains a property named matches which specifies whether or not the media query applies to the current page.

➡️ https://umaar.com/dev-tips/223-javascript-media-query-listener/
