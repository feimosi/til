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
