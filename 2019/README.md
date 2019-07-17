# TypeORM multiple ValueTransformers

```ts
import { ValueTransformer } from "typeorm";

export const lowercase: ValueTransformer = {
    to: (entityValue: string) => { return entityValue.toLocaleLowerCase() },
    from: (databaseValue: string) => { return databaseValue }
}

export const encrypt: ValueTransformer = {
    to: (entityValue: string) => { return crypto.encrypt(entityValue) },
    from: (databaseValue: string) => { return crypto.decrypt(databaseValue) }
}
```

```ts
@Column({ unique: true, transformer: [lowercase, encrypt] })
email: string;
```

https://github.com/typeorm/typeorm/issues/4007

# TypeScript utility type that strips away readonly

```ts
type Writable<T> = {
    -readonly [K in keyof T]: T[K]
}

// { a: string, b: number }
type A = Writable<{
    readonly a: string;
    readonly b: number
}>;
```

https://devblogs.microsoft.com/typescript/announcing-typescript-3-4/
