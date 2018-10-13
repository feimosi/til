<h1 align="center">14.10.2018</h1>

## Typescript: Create type with properties of another type

For every property named K in T, produce a new property of that name with the type string.

```ts
type Stringify<T> = {
    [K in keyof T]: string
};
```
