---
title: "Houdini Packed Prims"
categories:
  - blog
tags:
  - houdini
  - packed
---

# Houdini Packed Prims

## Replace packed prims

After using `copy to points` you can switch the geometry out, perhaps with variations or switching low res to high res.
Use a point wrangle and `getpackedtransform()` to extract the source and apply it to a newly created bunch of `copy to points`

![image](https://user-images.githubusercontent.com/12150445/151204761-b018d095-bb92-4af7-ab37-1d90141e2d96.png)

```c
// get packed transform from source
matrix packed_transform = getpackedtransform(1, @primnum);

// apply to the replaced geo
setpackedtransform(0, @elemnum, packed_transform);
```
