---
title: Entry level Javascript weirdness
categories:
  - it-stuff
tags:
  - javascript
---

Javascript is weird when compared to other popular programming languages. If you search it on the Internet, there’re a lot of articles talking about the weirdness of Javascript. For example: https://github.com/denysdovhan/wtfjs and https://jsisweird.com/

Here are something make me feel Javascript is a quite special programming language:

- 1: Array supports basically any value as index

  Lots of other languages require array indices to be integers. But in Javascript, you can use any value as an array index. Letters, special characters, numbers, arrays, and even function.

```javascript
let ary = []
ary[‘(’] = 1
ary[1.1] = 1
ary[true] = 1
```

Lots of other programming languages do not allow non integer indices for array at all

- 2: Using multiple wrapped brackets to access array elements will give you the same result as a single bracket gives.

```javascript
let ary = [0, 1, 2]
ary[0] === ary[[0]] // true
ary[0] === ary[[[0]]] // true
```

I believe almost all the other languages will just throw errors when using brackets like that.

- 3: typeof NaN is number, NaN == NaN gives you false

“Not a Number” is a number, sounds funny, and if to check a value is NaN, built-in function isNaN must be used as == or === always return false. NaN makes Javascript more special.

- 4: 1 / 0 gives you Infinity, 0 / 0 gives you NaN

```javascript
1 / 0 // Infinity
0 / 0 // NaN
```

Other languages will probably just throw errors for dividing zero operation

- 5: new + function to create an object

Before class was introduced to Javascript, using constructor pattern to create an object in Javascript is new + function

```javascript
function Foo() {}
let foo = new Foo()
```

new keyword + function is special, I don’t think this is common in other programming languages
