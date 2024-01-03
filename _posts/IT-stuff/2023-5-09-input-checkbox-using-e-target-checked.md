---
title: Html input checkbox using e.target.checked
categories:
  - it-stuff
tags:
  - javascript
---

`e.target.value` could look familiar by almost all experienced web developers. That's commonly seen in the input text box event handler in reactjs code, .e.g onChange.

However, when it comes to an input checkbox, you probably don't want to play with `e.target.value`, as it'll be always a string value "on", which is basicaly useless. The property should be used is `e.target.checked`. In the case of jQuery, use `.is(':checked')`.
