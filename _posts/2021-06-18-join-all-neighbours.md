---
title: "Houdini vex connect all points"
categories:
  - Houdini
tags:
  - houdini
  - vex
  - create-geo
---

### Houdini vex connect all points

A bit of an exercise of making a wrangle version of 'connect adjacent points', to give myself more control.

Connecting different named points is possible like this:

![image](https://github.com/simonreeves/blog/assets/12150445/333016b7-211f-497b-afb2-d65621d0c7e7)


So I decided to explore having more control in vex, using a `point wrangle`:

![image](https://user-images.githubusercontent.com/12150445/144092737-57f97ec1-7ba8-47be-b0db-6fcedaabd59d.png)


```c
// target index is the geo at input 1
int target_index = 1;

// for each point, find all nearpoints on input 1, store in array
int near_pts[] = nearpoints(target_index, @P, chf('maxdist'));

foreach (int near_pt; near_pts) {

    // randomly decide to skip this combination of ptnum and pt
    if (rand(near_pt + @ptnum + chf('seed')) < chf('random_cull')) {
        continue;
    }

    // get position of the found point
    vector found_pos = point(target_index, 'P', near_pt);

    // create new point on this geo from the found one
    int found_point = addpoint(0, found_pos);
    
    // create new prim from this point to newly created point
    int new_prim = addprim(0, 'polyline', @ptnum, found_point);
}
```

note that these lines skip some connections for a quick way to vary, or not generate so much!
```c
    // randomly decide to skip this combination of ptnum and pt
    if (rand(pt + @ptnum + chf('seed')) < chf('random_cull')) {
        continue;
    }
```

### Collide with intersection geometry
Additionally with geo plugged into the third input we can check if it is blocking connections and end them at that stage instead.
```c
// target index is the geo at input 1
int target_index = 1;
int collision_index = 2;

// for each point, find all nearpoints on input 1, store in array
int near_pts[] = nearpoints(target_index, @P, chf('maxdist'));

foreach (int near_pt; near_pts) {

    // randomly decide to skip this combination of ptnum and pt
    if (rand(near_pt + @ptnum + chf('seed')) < chf('random_cull')) {
        continue;
    }
    
    // get position of the found point
    vector found_pos = point(target_index, 'P', near_pt);

    // get direction from point to its foundpoint
    vector dir = normalize(found_pos - v@P) * 10.0;

    // init variables
    vector intersect_pos;
    vector intersect_uvw;
    
    // find intersection with collision geo along direction from point 
    int intersected = intersect(collision_index, v@P, dir, intersect_pos, intersect_uvw);
    
    int found_point;

    // if there was no collision
    if (intersected == -1) {
        // create new point from other geo
        found_point = addpoint(0, found_pos);
    }
    else {
        // instead use the interesect point
        found_point = addpoint(0, intersect_pos);
    }
    
    // create new prim using those points
    int new_prim = addprim(0, 'polyline', @ptnum, found_point);
}
```


### Reference sources
  - [cgwiki - joy of vex](https://www.tokeru.com/cgwiki/index.php?title=JoyOfVex14)
  - [Houdini vex docs addprim](https://www.sidefx.com/docs/houdini/vex/functions/addprim.html)
