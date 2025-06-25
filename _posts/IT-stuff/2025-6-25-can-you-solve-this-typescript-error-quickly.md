---
title: can you solve this typescript error quickly
categories:
  - it-stuff
tags:
  - typescript
---

```typescript
type User = {
  name: string
  age?: number
  active?: boolean
}

type UserArray = Array<User | null>

function activateUsers(users: UserArray): UserArray {
  return users.map((user) => ({
    ...user,
    active: true,
  }))
}
```

Looks all good? But if you test the code above, it will give you a lint error:

```
Type '{ active: true; name?: string | undefined; age?: number | undefined; }[]' is not assignable to type 'UserArray'.
  Type '{ active: true; name?: string | undefined; age?: number | undefined; }' is not assignable to type 'User'.
    Types of property 'name' are incompatible.
      Type 'string | undefined' is not assignable to type 'string'.
        Type 'undefined' is not assignable to type 'string'.typescript(2322)
```

Why is the error?
Please note one of properties of `User`: `name` is not nullable. The spread operation, `...user`, creates an intermediate type `{ active: true; name?: string | undefined; age?: number | undefined; }`, `name` is nullable, which is not compatible to `User | null`, so it cannot be mapped to build an array of `User | null`.

How to solve the error?
There're a few ways.

- 1: remove the nullable element

As the element of array is nullable, spread operation will create an intermediate type of which every property is nullable. Yes, that makes sense. Properties of a nullable value of course should still be nullable. So one simple way to fix the error is just to make the element of array not nullable: `type UserArray = Array<User>`

- 2: Make all User's properties nullable

Either change the name property of `User` to be nullable `name?: string`, or use `Partial`: `type UserArray = Array<Partial<User> | null>`

- 3: Add null checking at runtime

```
  return users.map((user) =>
    user ? { ...user, active: true } : null
  )
```

- 4: `as` operator helps as well, but it's not recommended due to lack of type-safe

```
  return users.map(
    (user) =>
      ({
        ...user,
        active: true,
      } as User)
  );
```
