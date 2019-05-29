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
