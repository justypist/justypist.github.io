# Typescript

```ts
type Keys<T> = keyof T;

type Values<T> = T extends {
  [key in keyof T]: infer P
} ? P : never;
```

```ts
type ReadonlyMock<T> = {
  readonly [key in keyof T]: T[key]
}

type WritableMock<T> = {
  -readonly [key in keyof T]: T[key]
}

type PartialMock<T> = {
  [key in keyof T]?: T[key]
}

type RequiredMock<T> = {
  [key in keyof T]-?: T[key]
}
```

```ts
type PickMock<T, Keys extends keyof T> = {
  [Key in Keys]: T[Key]
}

type OmitMock<T, Keys extends keyof T> = {
  [k in keyof T as k extends Keys ? never : k]: T[k]
}

type ExcludeMock<T, Keys> = T extends Keys ? never : T;
```

```ts
type ExtractMock<T, Key> = Key extends T ? Key : never;

type NonNullableMock<T> = T extends (null | undefined) ? never : T;

type RecordMockKey = string | number | symbol;
type RecordMock<Keys extends RecordMockKey,V> = {
  [k in Keys]: V
}
```

```ts
type ConstructorParametersMock<T> = T extends { new (...args: infer P): any } ? P : never;

type ParameterMock<T extends Function> = T extends (...arg: infer P) => any ? P : never;

type ReturnTypeMock<T extends Function> = T extends (...obj: any[]) => infer P ? P : never;
```
