---
title: Three ways of defining a generic function type using typescripts
categories:
  - it-stuff
tags:
  - typescript
---

If to pass a variable which is a function, and also the type of that function need to be a generic one, it's useful to define a generic function type when using typescript.

For example, in order to to pass value to the callback, how to define `GenericFunctionType`?

```typescript
interface Foo<T> {
  foo(value: T, callback: GenericFunctionType<T, unknown>): void
}
```

- 1: use `type` plus curly brackets block

```typescript
type GenericFunctionType<TInput, TReturn> = {
  (args: TInput): TReturn
}
```

- 2: use `type` plus arrow function style

```typescript
type GenericFunctionType<TInput, TReturn> = (args: TInput) => TReturn
```

- 3: use interface plus curly brackets block

```typescript
interface GenericFunctionType<TInput, TReturn> {
  (args: TInput): TReturn
}
```

Finally, to finish the example `interface Foo` by implementing it:

```typescript
class Coo<T> implements Foo<T> {
  foo(value: T, callback: GenericFunctionType<T, unknown>) {
    callback(value)
  }
}

const sf = new Coo<string>()
sf.foo('Hello world', (v) => console.log(v))
const nf = new Coo<number>()
nf.foo(123, (v) => console.log(v))
const bf = new Coo<boolean>()
bf.foo(true, (v) => console.log(v))
```

Output:

```text
[LOG]: "Hello world"
[LOG]: 123
[LOG]: true
```
