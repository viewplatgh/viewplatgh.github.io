---
layout: post
title: CSS justify-* align-* two things to remember at first
categories:
  - it-stuff
tags:
  - css
  - flexbox
---

CSS justify-\* align-\* basically includes: `justify-content`, `justify-item`, `justify-self`, `align-content`, `align-item` and `alignt-self`

When first time to learn about these properties, it can be overwhelming and hard to grasp.

It should be useful to remember two things about them before trying to master at them.

1. `justify-item`, `justify-self` are not for flex, don't play with them in flex, on the other hand, they're often used in grid.

2. `align-content` is not for `no-wrap` in flex, i.e. it only works when `flex-wrap` is set to `wrap` or `wrap-reverse`.
