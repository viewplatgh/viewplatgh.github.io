---
title: An interesting react typescript destructuring syntax, kebab naming property with a default value
categories:
  - it-stuff
tags:
  - react
  - typescript
  - javascript
---

```typescript
type ComponentProps = React.HTMLProps<HTMLDivElement>;

export default function Component({
  "aria-busy": ariaBusy = true,
}: ComponentProps) {
  return (
    <div ariaBusy={ariaBusy}>
      <h1>Interesting react typescript destructuring syntax</h1>
    </div>
  );
}
```

First glance at the code above, you could get confused with the syntax `{ "aria-busy": ariaBusy = true }` where the component trying to destructure props passed from parent components.

Here is the explain:
1. if `props` has a property name using kebab style, e.g. "aria-busy", the syntax to destructure it is to use quotes to wrap the property: `{ "aria-busy": ariaBusy }`, because without the quotes, ts/js interpreter would believe the dash '-' is a subtraction operator, and wouldn't understand the way it's used.

2. setting a default value for destructured property is supported in ts/js, the syntax to do it is just to put a default value like `= true` at right side of the variable.

In a nutshell, `{ "aria-busy": ariaBusy = true }` is a valid destructuring syntax supported by modern ts/js.