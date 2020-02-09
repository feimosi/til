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
