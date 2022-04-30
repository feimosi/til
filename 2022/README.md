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
