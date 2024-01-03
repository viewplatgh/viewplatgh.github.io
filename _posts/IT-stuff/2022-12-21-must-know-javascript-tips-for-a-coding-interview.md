---
title: Must know Javascript tips for a coding interview
categories:
  - it-stuff
tags:
  - javascript
---

1. Generate an array

   To create 12 elements array, don't do:

   ```javascript
   let a12 = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11];
   ```

   Do:

   ```javascript
   let a12 = Array.from({ length: 12 }, (_, idx) => idx); // (12) [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
   ```

   And the way to generate an array with single value:

   ```javascript
   let a12 = Array(12).fill(0); // (12) [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
   ```

2. Convert a string to an array

   To convert 'abc' to ['a', 'b', 'c'], use `split`

   ```javascript
   'abc'.split(''); // (3) ['a', 'b', 'c']
   ```

3. Join string array back to string

   To convert ['a', 'b', 'c'] to 'abc', use `join`

   ```javascript
   ['a', 'b', 'c'].join(''); // 'abc'
   ```
