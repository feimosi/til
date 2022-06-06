# [TypeScript] Indexed access type

```ts
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
