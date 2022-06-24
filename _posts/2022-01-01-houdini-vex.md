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

## Find first of an attribte and make point
Use a fresh wrangle and create new points from second index, instead of 'keeping' some points from the input.

![image](https://user-images.githubusercontent.com/12150445/175514915-df63a713-f52a-4a88-8b30-f45e2bbe648c.png)

Counts the unique values of an attribute `neb_id` then loops through them to find the first point with that value.

```c
// in a detail wrangle
int keepers[];

// get the unique values of an attribute
int neb_ids[] = uniquevals(1, 'point', 'neb_id');

// loop through each value
foreach (int neb_id; neb_ids) {
    // find the first matching point
    int found = findattribval(1, "point", "neb_id", neb_id);
    // append to an array
    append(keepers, found);
 }

// create new points from the matching points
 foreach (int foundpt; keepers) {
    vector pos = point(1, 'P', foundpt);
    addpoint(0, pos);
}
```

Instead of just fetching the firt found point, choose how many to keep from each unique val with `int keep_num = chi('amount_to_keep');`

```c
// in a detail wrangle
int keepers[];
int keep_num = chi('amount_to_keep');
int neb_ids[] = uniquevals(1, 'point', 'neb_id');

foreach (int neb_id; neb_ids) {
    for (int w=0; w<keep_num; w++) {
        int found = findattribval(1, "point", "neb_id", neb_id, w);
        append(keepers, found);
    }
 }

 foreach (int foundpt; keepers) {
    vector pos = point(1, 'P', foundpt);
    addpoint(0, pos);
}
```