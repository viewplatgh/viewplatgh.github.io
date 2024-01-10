---
title: Simple shortid, uuid (unique id) solution in Javascript
categories:
  - it-stuff
tags:
  - javascript
---

When working for a company or a serious side hustle, probably this should be done via a third party library like <https://www.npmjs.com/package/nanoid>. However, when just to create some code snippets or some example projects for whatever reason, it's handy to know and worth to momerise following simple way to generate short unique ids.

```javascript
Math.random().toString(36).slice(2)
```

This one liner solution is borrowed from: <https://stackoverflow.com/questions/6248666/how-to-generate-short-uid-like-ax4j9z-in-js>, credits to Martin Braun. I like elegent one liner solutions. Just I changed the argument passed to slice from -6 to 2, which makes it generate a bit longer string (length 11) than shorter one (length 6).
