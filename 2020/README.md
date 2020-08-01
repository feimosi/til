# String.replace() replacer function

`replace()` also accepts a replacer function instead of a replacement text:

```js
"\t\tsomething\t".replace(
  /^\t+/,
  (tabs) => tabs.
    replace(
      /./g,
      "&nbsp;&nbsp;"
    )
)
```

# `index` argument in Lodash FP

Functions such as `map, filter, and reduce` get `(element, index, array)` but with Lodash FP they are capped to only one argument.

Use convert to remove this capping:

```js
_.map.convert({cap: false})((e, i) => i + 1)([1, 2]); // [1, 2]
```

# Sorting strings containing numbers

The issue:

```js
["item11", "item10", "item9"].sort() // ["item10", "item11", "item9"]
```

Apart from lexical sorting, Javascript supports comparison using locales, and that supports handling numbers in a more natural way.

Numeric-aware sorting is done by using `localeCompare`, and passing `{ numeric: true }` to the options, the third argument:

```js
["item11", "item10", "item9"]
    .sort((a, b) => a.localeCompare(b, undefined, { numeric: true }));
    // ["item9","item10","item11"]
```

For reusability use a `Collator`:

```js
const collator = new Intl.Collator(undefined, { numeric: true });

["item11", "item10", "item9"].sort(collator.compare) // ["item9","item10","item11"]
```

# Bind custom function arguments

If you want the second etc. argument to be bound, native `bind` provides little help.

However Lodash provides a `bind` function. Its signature is similar to the native `bind`, but it's more generic, as it supports `_.bind.placeholder` to skip parameters. Using this placeholder, any of the parameters can be partially applied.

```js
const powerOf2 = _.bind(Math.pow, undefined, _.bind.placeholder, 2);

console.log(powerOf2(3)); // 3^2 = 9
```

# TypeScript: Type predicates

Type predicates in TypeScript help you narrowing down your types based on conditionals. They’re similar to type guards, but work on functions. They way the work is, if a function returns true, change the type of the paramter to something more useful.

```ts
function isString(s): s is string {
  return typeof s === 'string';
}

function toUpperCase(x: unknown) {
  if(isString(x)) {
    x.toUpperCase(); // ✅ all good, x is string
  }
}
```

```ts
type Dice = 1 | 2 | 3 | 4 | 5 | 6;

function pipsAreValid(pips: number): pips is Dice {
  return pips === 1 || pips === 2 || pips === 3 ||
    pips === 4 || pips === 5 || pips === 6;
}

function evalThrow(count: number) {
  if (pipsAreValid(count)) {
    // count is now of type Dice 😎
    // ...
  }
}
```

:arrow_right: https://fettblog.eu/typescript-type-predicates/

# TypeScript: Assertion signatures

```ts
function addCache<T extends {}>(
  service: T,
): asserts service is T & { cache: Cache } {
  Object.defineProperty(service, 'cache', new Cache());
}

const someService = {};

addCache(someService);

someService.cache instanceof Cache; // => true
```

:arrow_right: https://fettblog.eu/typescript-assertion-signatures/

# TypeScript: Exhaustiveness checking

We would like the compiler to tell us when we don’t cover all variants of the discriminated union. For example, if we add Triangle to Shape:

```ts
type Shape = Square | Rectangle | Circle | Triangle;

function area(s: Shape) {
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.height * s.width;
    }
    // should error here - we didn't handle case "triangle"
}
```

Use the `never` type that the compiler uses to check for exhaustiveness

```ts
function area(s: Shape) {
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.height * s.width;
        default: return assertNever(s); // error here if there are missing cases
    }
}

function assertNever(x: never): never {
    throw new Error("Unexpected object: " + x);
}
```

:arrow_right: https://www.typescriptlang.org/docs/handbook/advanced-types.html#exhaustiveness-checking

# Get an event's coordinates relative to an element

The MouseEvent object has several properties that indicate the position where the event happened:

- `offsetX/Y` have the coordinates relative to the target element
- `pageX/Y` are to the document
- `screenX/Y` are to the screen
- `clientX/Y` (aka. x/y) are to the viewport

To get the coordinates relative to another element, the simplest solution is to use `.getBoundingClientRect()` which uses viewport coordinates, the same as `clientX/Y`.

```js
const { top, left } = element.getBoundingClientRect();

const eventTop = event.clientY - top;
const eventLeft = event.clientX - left;
```

# React hook to persist state in localStorage

```js
function useStickyState(defaultValue, key) {
  const [value, setValue] = React.useState(() => {
    const stickyValue = window.localStorage.getItem(key);
    return stickyValue !== null
      ? JSON.parse(stickyValue)
      : defaultValue;
  });
  React.useEffect(() => {
    window.localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);
  return [value, setValue];
}

...

const CalendarView = () => {
  const [mode, setMode] = useStickyState('day', 'calendar-view');
  // ...
}
```

:arrow_right: https://joshwcomeau.com/react/persisting-react-state-in-localstorage/

# `componentDidMount` vs `useEffect` 

`componentDidMount` and `useEffect` run after the mount. However `useEffect` runs after the paint has been committed to the screen as opposed to before. This means you would get a flicker if you needed to read from the DOM, then synchronously set state to make new UI.

`useLayoutEffect` was designed to have the same timing as `componentDidMount`. So `useLayoutEffect(fn, [])` is a much closer match to `componentDidMount()` than `useEffect(fn, [])` -- at least from a timing standpoint.

## `useEffect` cleanup

Don't forget to do a cleanup function - this will prevent the bug of setting state on an unmounted component and setting stale state when the uid changes:

```js
useEffect(() => {
  let isCurrent = true
  getUser(uid).then(user => {
    if (isCurrent) {
      setUser(user)
    }
  })
  return () => {
    isCurrent = false
  }
}, [uid])
```

:arrow_right: https://reacttraining.com/blog/useEffect-is-not-the-new-componentDidMount

# PostgreSQL: Populate table with large dataset

One million records are inserted in the following example:

```sql
INSERT INTO demotable SELECT random() * 1000,  generate_series(1, 1000000);
```

# `Promise.allSettled`

`Promise.allSettled` gives you a signal when all the input promises are settled, which means they’re either fulfilled or rejected. This is useful in cases where you don’t care about the state of the promise, you just want to know when the work is done, regardless of whether it was successful.

```js
const promises = [
  fetch('/api-call-1'),
  fetch('/api-call-2'),
  fetch('/api-call-3'),
];
// Imagine some of these requests fail, and some succeed.

await Promise.allSettled(promises);
// All API calls have finished (either failed or succeeded).
removeLoadingIndicator();
```

# `String.matchAll`

A common use case of global (g) or sticky (y) regular expressions is applying it to a string and iterating through all of the matches. The new `String.matchAll` API makes this easier than ever before, especially for regular expressions with capture groups:

```js
const string = 'Favorite GitHub repos: tc39/ecma262 v8/v8.dev';
const regex = /\b(?<owner>[a-z0-9]+)\/(?<repo>[a-z0-9\.]+)\b/g;

for (const match of string.matchAll(regex)) {
  console.log(`${match[0]} at ${match.index} with '${match.input}'`);
  console.log(`→ owner: ${match.groups.owner}`);
  console.log(`→ repo: ${match.groups.repo}`);
}
```

# TypeScript: Discriminated Unions

TypeScript is structurally typed, it only cares about the properties that objects contain (not how they were constructed).

There are 2 requirements for discriminated unions:

- Types that have a common, singleton type property — the discriminant.
- A type alias that takes the union of those types — the union.

```ts
interface Dog {
    kind: "dog"
    bark: string
}

interface Cat {
    kind: "cat"
    meow: string
}

type Animal = Cat | Dog
```

Now given an `Animal`, we can use the `kind` property to distinguish which one we have. For example

```ts
function animalNoise(animal: Animal): string {
    // TypeScript won't allow us to access bark or meow because this animal might not have them

    if (animal.kind == "dog") {
        // Now TypeScript knows the animal is a Dog, so we can safely access bark
        return animal.bark
    } else {
        // Here the compiler has determined the animal must be a Cat
        return animal.meow
    }
}
```

# TypeScript: `Parameters<T>`

A base TypeScript utility type that returns a tuple of the parameter types for given function.

```ts
function foo(a: string, b: number): void {} // (a: string, b: number): void

Parameters<typeof foo> // [string, number]

function bar(a: Parameters<typeof foo>[0]) {} // (a: string): void
```

# Meta Refresh

Redirects to the provided URL in 5 seconds. Set to 0 for an immediate redirect

```html
<meta http-equiv="refresh" content="5">
```

# Git: `push --force-with-lease`

`--force-with-lease` is a safer option that will not overwrite any work on the remote branch if more commits were added to the remote branch. It ensures you do not overwrite someone elses work by force pushing.

# TypeScript 

## Declaration merging, module augmentation

```ts
interface Foo { foo: string }
interface Foo { bar: string }

// Interfaces merge
const foo: Foo = { 
  foo: 'hello',
  bar: ‘world’,
}

// Types aliases don’t.  Error: “Duplicate identifier 'Foo'.”
type Foo = { foo: string }
type Foo = { bar: string } // Error
```

## Index signature

Interfaces need an explicit index signature while types have an implicit one.

```ts
interface MyStyles {
  container: string;
  wrapper: string;
  footer: string;
}

declare const myStyles: MyStyles;
declare function inject(styles: Record<string, string>): void;

inject(myStyles); // Error: "Index signature is missing in type 'MyStyles'.
```

```ts
interface MyStyles {
  container: string;
  wrapper: string;
  footer: string;
  [index: string]: string;
}

type MyStyles = {
  container: string;
  wrapper: string;
  footer: string;
}
```

## Interfaces vs Types

Use interfaces where you can.

- They look better in error messages (although it might be subjective)
- They require explicit index signatures
- Extending is better than intersections (you cannot accidentally override a property). By doing intersections you might accidentally run into a situation where a value must of of empty type like `string & number`.
- They can be augmented when necessary (good for library authors)
- Smaller memory footprint (because they are lazy and cacheable)

:arrow_right: https://paper.dropbox.com/doc/WTF-TypeScript--Axf2A4Kth5vPl9RXElMtHExCAg-fyxWXDfqYssUzsAzmGmZL

# React `useCallback`

Apply `useCallback()` to preserve the callback instance between renderings

```jsx
const MemoizedLogout = React.memo(Logout);

function MyApp({ store, cookies }) {
  const onLogout = useCallback(
    () => cookies.clear('session'), 
    [cookies]
  );

  return (
    <div className="main">
      <header>
        <MemoizedLogout
          username={store.username}
          onLogout={onLogout}
        />
      </header>
      {store.content}
    </div>
  );
}
```

# TypeScript

## Object vs object

`Object` is the type of all instances of class `Object` (includes primitive values, describes functionality that is common to all JavaScript objects).

`object` is the type of all non-primitive values.

:arrow_right: https://github.com/David-Else/modern-typescript-with-examples-cheat-sheet#object-versus-object

## Interface Signatures Overview

```ts
interface ExampleInterface {
  myProperty: boolean; // Property signature
  myMethod(x: string): void; // Method signature
  [prop: string]: any; // Index signature
  (x: number): string; // Call signature
  new (x: string): ExampleInstance; // Construct signature
}
```

**Index Signature** helps to describe Arrays or objects that are used as dictionaries.

**Call Signature** enables interfaces to describe functions, `this` is the optional calling context of the function.

:arrow_right: https://github.com/David-Else/modern-typescript-with-examples-cheat-sheet#interface-signatures-overview

## Mapped Types

### `typeof`

```ts
const data = ["text 1", "text 2"] as const;
type Data = typeof data[number]; // "text 1" | "text 2"
```

```ts
const locales = [
  {
    locale: "se",
    language: "Swedish"
  },
  {
    locale: "en",
    language: "English"
  }
] as const;
type Locale = typeof locales[number]["locale"]; // "se" | "en"
```

### `keyof`

```ts
const currencySymbols = {
  GBP: "£",
  USD: "$",
  EUR: "€"
};
type CurrencySymbol = keyof typeof currencySymbols; // "GBP" | "USD" | "EUR"
```

`keyof` with generics and interfaces example:

```ts
interface HasPhoneNumber {
  name: string;
  phone: number;
}

interface HasEmail {
  name: string;
  email: string;
}

interface CommunicationMethods {
  email: HasEmail;
  phone: HasPhoneNumber;
  fax: { fax: number };
}

function contact<K extends keyof CommunicationMethods>(
  method: K,
  contact: CommunicationMethods[K] // turning key into value - a mapped type
) {
  //...
}
contact("email", { name: "foo", email: "mike@example.com" });
contact("phone", { name: "foo", phone: 3213332222 });
contact("fax", { fax: 1231 });
```

We can get all values by mapping through all keys:

```ts
type AllCommKeys = keyof CommunicationMethods;
type AllCommValues = CommunicationMethods[keyof CommunicationMethods];
```

:arrow_right: https://github.com/David-Else/modern-typescript-with-examples-cheat-sheet#mapped-types---getting-types-from-data

## readonly Array / Tuple

```ts
const array: readonly string[];
const tuple: readonly [string, string];
```

## `const` assertions

- `number` becomes number literal: `let x = 10 as const`
- array literals become `readonly` tuples: `let y = [10, 20] as const`
- object literals get `readonly` properties

:arrow_right: https://github.com/David-Else/modern-typescript-with-examples-cheat-sheet#const-assertions

## `never`

`never` represents the type of values that never occur. It is used in the following two places:

- as the return type of functions that never return
- as the type of variables under type guards that are never true

```ts
function controlFlowAnalysisWithNever(value: string | number) {
  if (typeof value === "string") {
    value; // Type string
  } else if (typeof value === "number") {
    value; // Type number
  } else {
    value; // Type never
  }
}
```

:arrow_right: https://github.com/David-Else/modern-typescript-with-examples-cheat-sheet#never

## `unknown`

`unknown` is the type-safe counterpart of the `any` type: we have to do some form of checking before performing most operations on values of type unknown.

:arrow_right: https://github.com/David-Else/modern-typescript-with-examples-cheat-sheet#unknown

## Advanced Factory using `ConstructorParameters<T>` and `InstanceType<T>`

```ts
class Hero {
  constructor(public point: [number, number]) {}
}

const entities = [];

const entityFactory = <
  T extends {
    new (...args: any[]): any;
  }
>(
  classToCreate: T,
  numberOf: number,
  ...args: ConstructorParameters<T>
): InstanceType<T>[] =>
  [...Array(numberOf)].map(() => new classToCreate(...args));

entities.push(...entityFactory(Hero, 10, [12, 10]));
```

## Discriminated Unions

A data structure used to hold a value that could take on several different, but fixed, types.

## Assertion function - check for a condition

```ts
function assert(condition: any, msg?: string): asserts condition {
  if (!condition) {
    throw new AssertionError(msg);
  }
}

function yell(str) {
  assert(typeof str === "string");

  return str.toUppercase();
  //         ~~~~~~~~~~~
  // error: Property 'toUppercase' does not exist on type 'string'.
  //        Did you mean 'toUpperCase'?
}
```

:arrow_right: https://github.com/David-Else/modern-typescript-with-examples-cheat-sheet#assertion-function-style-1---check-for-a-condition

# `textContent` vs `innerText`

1. `innerText` was non-standard, while `textContent` was standardized earlier.
2. `innerText` returns the visible text contained in a node, while `textContent` returns the full text. 
For example `<span>Hello <span style="display: none;">World</span></span>`, 
  - `innerText` will return 'Hello', 
  - `textContent` will return 'Hello World'.
3. As a result, `innerText` is much more performance-heavy: it requires layout information to return the result.
4. `innerText` is defined only for `HTMLElement` objects, while `textContent` is defined for all `Node` objects.
5. `innerText` will turn `<br>` elements into newline characters, while `textContent` will just ignore them. So 2 words with only a `<br>` element between them (and no spaces) will be concatenated when using `textContent`.
6. If you change the `text-transform` of an element by CSS, it will affect the result of `innerText`, but not the result of `textContent`. For example `<div style="text-transform: uppercase;">Hello World</div>` 
  - `innerText`will be "HELLO WORLD", 
  - `textContent` will be "Hello World"

## Convert images to PDF

```sh
convert -quality 70 *.jpg document.pdf
```
