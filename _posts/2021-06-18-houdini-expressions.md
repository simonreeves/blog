---
title: "Houdini Expressions"
categories:
  - blog
tags:
  - houdini
  - expressions
---

### Rotate camera 360 degress in 100 frames

Frame `1101` will match `1001` so `1001-1100` is the loop
```c
fit(@Frame, 1001, 1101, 0, 360)
```