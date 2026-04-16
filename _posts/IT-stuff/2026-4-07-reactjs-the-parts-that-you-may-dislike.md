---
title: ReactJS, the parts that you may dislike
categories:
  - it-stuff
tags:
  - reactjs
---

ReactJS is one of the most popular frontend libraries, but everything has its downside. In this post, I'd like to share some reasons why people might thumb-down ReactJS. This can also be useful for answering a common frontend engineer interview question: what are the cons of ReactJS?

- 1: ReactJS is heavy

  Here is a simple comparison of package size with other popular frameworks and libraries:
  - Svelte 5 (~2-3 KB)
  - SolidJS 1.9 (~7.6 KB)
  - jQuery 4.0 (~27.6 KB)
  - Vue 3.5 (~34 KB)
  - React 19 + ReactDOM (~45 KB)

  A larger package means more cost to ship, build and run the application using it.

- 2: The learning curve is steep

  There are two component systems you need to learn. Although class components are no longer recommended for building new apps, there are no plans to deprecate them either. Plenty of class component code still lives in both open source projects and proprietary codebases. On top of that, there are many other legacy APIs you need to be aware of, such as `forwardRef`, `cloneElement` and so on.

- 3: Lack of simplicity

  Even a simple backend interaction — fetch and update — is not straightforward. You either need to introduce a library like react-query, or fall back to the classic `useEffect` + `setState` pattern. `useState` and `useEffect` can be easily overused to handle any UI update, and you often end up with a bunch of them piled into a single component body.

- 4: Restrictions and limits
  - **Rules of hooks**: you cannot call hooks wherever you want, can only call hooks in function components body or custom hooks.
  - **Unique and stable Keys required for an array of elements**: This is easy to forget when doing day-to-day work and it could be distracting
  - **Fragments are required to wrap elements**: This is trivial but still some overhead
  - **Ref is tricky**: This got simpler after `forwardRef` was deprecated in React 19, but ref-related code can still look awkward and be hard to follow
  - **Controlled or not controlled, that's a question**: If to write a custom "form field" control, deciding whether to support only controlled mode or both can be a struggle

- 5: Pitfalls can catch you easily

  There are pitfalls that are error-prone, especially for engineers new to React. Infinite re-rendering caused by updating state in the wrong spot, or by a dependency loop. State duplication, often due to `useState` overuse. Too many state updates causing performance issues, often due to `useEffect` overuse. Frequent hydration errors when working with NextJS. The list goes on.
