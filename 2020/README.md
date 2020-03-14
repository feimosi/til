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
