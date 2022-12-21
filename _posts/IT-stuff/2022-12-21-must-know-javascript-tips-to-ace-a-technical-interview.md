---
layout: post
title: Must know Javascript tips to ace a technical interview
tag: it-stuff
---

1. Generate an array
   To create 12 elements array, don't do:

```javascript
let a12 = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11];
```

Do:

```javascript
let a12 = Array.from({ length: 12 }, (_, idx) => idx);
```

And the way to generate an array with single value:

```javascript
let a12 = Array(12).fill(0);
```

2. Convert a string to an array
   To convert 'abc' to ['a', 'b', 'c'], use `split`

```javascript
'abc'.split('');
```

3. Join string array back to string
   To convert ['a', 'b', 'c'] to 'abc', use `join`

```javascript
['a', 'b', 'c'].join('');
```
