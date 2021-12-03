---
title: "Nuke Expression Node"
categories:
  - blog
tags:
  - nuke
  - expressions
classes: wide
---


### Offset Image
Use the `expression` node to offset pixels, for instance to make sure an image is tileable

In each channel R/G/B
```
r((x+offset)%width, y)
g((x+offset)%width, y)
b((x+offset)%width, y)
```
  - `r` would get the red value of that pixel, equivilant to `r(x, y)`
  - To get a different pixel `r(50,245)`
  - `offset` refers to the custom variable set at the top of the node


![image](https://user-images.githubusercontent.com/12150445/144228352-80b348d4-4e4d-4088-bde2-62b12627d04a.png)
