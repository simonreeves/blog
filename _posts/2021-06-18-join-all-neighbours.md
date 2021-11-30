---
title: "Houdini vex connect all points"
categories:
  - blog
tags:
  - houdini
  - vex
  - create-geo
---

### Houdini vex connect all points

I have two objects, I want to 'connect adjacent points' but I find that node wont just connect EVERYTHING, not sure where it limits it..


So in vex in a `point wrangle`:

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
