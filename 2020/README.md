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
