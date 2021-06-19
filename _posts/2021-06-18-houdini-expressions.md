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

### Link Camera DOF to Null
Make a `null` object as a sibling to the camera, name it 'focus' and use this expression in the camera's focal distance parm to get the distance between the two objects

```c
vlength(vtorigin(".","../focus"))
```


### Frame Sequnce offset
We always start frameranges from `1001` instead of `1` to make it a little easier when dealine with preroll - so often one needs to offset an image sequence from 1 to 1001
For example in the background of a camera:

Subtract 1000 frames but *importantly* add the frame padding again!
```c
$JOB/MySequence.`padzero(4, $F4-1000)`.jpg
```