<h1 align="center">14.10.2018</h1>

## Typescript: Create type with properties of another type

For every property named K in T, produce a new property of that name with the type string.

```ts
type Stringify<T> = {
    [K in keyof T]: string
};
```

<h1 align="center">20.10.2018</h1>

## Enable diffing of untracked files

```git
git add -N
# or
git add --intent-to-add
```

<h1 align="center">21.10.2018</h1>

## Vim

### Paste and move the cursor after the pasted text

```vi
gp
```

### Delete whole word

```vi
daw
```
