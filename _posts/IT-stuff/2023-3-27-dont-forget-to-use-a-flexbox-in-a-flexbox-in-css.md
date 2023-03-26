---
layout: post
title: Don't forget to use a flexbox in a flexbox in CSS
tag: it-stuff
---

Millions tutorials online of CSS flexbox is about one pattern: parent `dispay: flex;` + one level children. However, in practice, flexbox is used in nested pattern more often than not. That is grandparent `display: flex;` + parent `display: flex;` + grandchildren. It's very useful to know that you don't have to use flexbox in only one level.

The typical case is that you want to have a responsive grid, and for each cell of the grid, you want to center an element, either a picture or some text, e.g.

HTML:

```html
<div class="grid">
  <!-- ... can be any html elements with different size -->
  <div class="cell">...</div>
  <div class="cell">...</div>
  <div class="cell">...</div>
  <div class="cell">...</div>
  <div class="cell">...</div>
</div>
```

```css
.grid {
  display: flex;
  justify-content: center;
}

.cell {
  width: 240px;
  height: 240px;
  border: 1px solid black;
  display: flex;
  justify-content: center;
  align-items: center;
}
```

In addition, to add some space between each cell easily, there is a handy `gap` css attribute:

```css
.grid {
  display: flex;
  justify-content: center;
  gap: 1rem;
}
```
