---
title: "Houdini Vex"
categories:
  - blog
tags:
  - houdini
  - vex
---

# Houdini Vex
Lots of misc vex things!

## Add point on each prim
On a prim wrangle!
```c
addpoint(0, @P);
removeprim(0, @primnum, 1);
```
![image](https://user-images.githubusercontent.com/12150445/151206580-044b3158-104b-4861-bad4-64d0371237f6.png)

## Misc
```c
// get nearest point index of second input
int np = nearpoint(1, @P);

// get that point's position
vector np_pos = point(1, 'P', np);

// measure distance
float dist = distance(@P, np_pos);

// normalise distance
dist = fit(dist, ch('in_min'), ch('in_max'), ch('out_min'), ch('out_max'));

// remap with ramp
dist = chramp('remap', dist);

// set attribute
f@dist = dist;
```
## Get centroid
```c
// get centroid
vector min, max;
getbbox(min, max);
vector centroid = (min+max)/2;
```

## Create random direction vector in cone
```c
vector axis = {0, 1, 0};
vector2 u = rand(@class);
vector direction = sample_direction_cone(axis, radians(ch("angle")), u);
```
