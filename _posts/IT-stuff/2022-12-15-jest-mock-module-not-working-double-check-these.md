---
layout: post
title: Jest mock module not working? double check these
tag: it-stuff
---

Jest mock module feature can be used for mocking whole or part of packages in your project. Check out: [https://jestjs.io/docs/manual-mocks](https://jestjs.io/docs/manual-mocks)

If you try `jest.mock('my-package')` first time, it may not work, and it just used the actual package without giving a hint of what's wrong.

Here is three typical mistakes cause that:

### jest.mock must be called at global scope, if you wrap it in a function and then call it, it won't work.

```javascript
// Wrong
function mockMe() {
  jest.mock('my-package');
}
mockMe();
```

```javascript
// Correct
jest.mock('my-package');
```

### Forgot to use `__esModule: true` setting

If your packages are written in ESM syntax, make sure \_\_esModule: true setting is included

```javascript
jest.mock('../myModule', () => {
  // Require the original module to not be mocked...
  const originalModule = jest.requireActual('../myModule');

  return {
    __esModule: true, // Use it when dealing with esModules
    ...originalModule,
    getRandom: jest.fn(() => 10),
  };
});
```

### Double check the object you're mocking is really in the package

Take the code snippet above as an example, make sure 'getRandom' is actually exported from `../myModule` rather than somewhere else.
