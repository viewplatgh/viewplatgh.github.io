---
title: typescript/javascript function can use returned value in arguments
categories:
  - it-stuff
tags:
  - typescript
  - javascript
---

```typescript
function foo({ handler }: { handler: () => void }) {
  handler()
  return {
    bar() {
      return 'the string bar'
    },
  }
}

const { bar } = foo({ handler: () => console.log('calling bar: ', bar()) })
```

You don't know javascript another time.

Look, the arguments of foo is using it's returned value, looks like a snake eating its own tail, isn't it? But eslint doesn't complain anything about it. It really is a piece of valid typescript/javascript code!

However, if you run it, it throws an error: `TypeError: bar is not a function`. It's easy to fix though, by just replacing `handler()` with `setTimeout(handler, 0);` The console would log: `calling bar:  the string bar` successfully.

Here is the explanation:

1. simply using returned value in arguments like `const { bar } = foo(bar)` is not valid, as when calling `foo`, `bar` is not ready for use at all. However, using returned value in an inline function is OK. Beacause, that inline function can be called long after `foo` finishing, when `bar` would be ready to use for long.

2. `bar` is not ready yet if to call `handler` immediately in `foo`, that's why it throws a runtime error. But it's all good to call `handler` in another round event loop. e.g. using `setTimeout` to call `handler` asynchronously.
