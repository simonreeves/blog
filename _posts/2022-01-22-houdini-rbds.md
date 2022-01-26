---
title: "Houdini RBDs"
categories:
  - blog
tags:
  - houdini
  - rigid body
  - rbds
  - dynamics
  - simulation
  - houdini 18.5
---

# RBDs

## Cache points only
Useful big save on caching to disk is just saving the points from a RBD solver. But this often doesn’t work how you would expect.

This example works, I had problems with using the input before the solve or configure geo, but the first frame of the solver works fine - and in fact if you look inside the RBD solver SOP, it is actually using transform pieces for the first output, so you know its legit…

![image](https://user-images.githubusercontent.com/12150445/151206026-bcc30c78-640c-4b02-a3a6-fc7e71f1c91d.png)

