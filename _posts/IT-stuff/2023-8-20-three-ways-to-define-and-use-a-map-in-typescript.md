---
layout: post
title: Three ways to define and use a Map in Typescript
tag: it-stuff
---

I am not sure how many ways exactly to use a Map in Typescript. Just to list three here I believe are most commonly seen.

- 1: use `[key: stirng]: any`

Javascript objects can be used as Map straight away. We can assign as many as key-value pairs we want into a Javascript object and then access values using square bracket notation. When using Typescript, we need to define a type for it:

```typescript
type MyMap = {
  [key: string]: any
}
const
```

This is to define a Map type using string value as keys, and values can be any type

```typescript
const myMap: MyMap = new Object() as MyMap
myMap['name'] = 'Jack'
myMap['age'] = 25
```

- 2: Use `Record<string, any>`

Instead of creating a Map type manually, we can borrow Typescript built-in utility Record

```typescript
const myMap: Record<string, any> = new Object() as Record<string, any>
myMap['name'] = 'Jack'
myMap['age'] = 25
```

- 3: Use built-in Map

```typescript
const myMap: Map<string, any> = new Map<string, any>()
myMap.set('name', 'Jack')
myMap.get('name')
```

One of the advantages of using built-in Map is the convenience of built-in Map methods such as `get`, `set` and `clear` etc.
