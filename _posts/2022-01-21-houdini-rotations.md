---
title: "Houdini Rotations"
categories:
  - blog
tags:
  - houdini
  - expressions
  - rotation
---

### Rotate Orient Attribute
I created orient attributes on points already, and now wanted them to spin randomly. The points are fed into `copy to points`.
I could 'rotate packed prims' afterwards but they are not packed prims yet as I was using redshift proxies so want to rotate the `@orient` instead.
Blatently foun info from (cgwiki 🙏)[https://www.tokeru.com/cgwiki/index.php?title=JoyOfVex17#Combine_orients_with_qmultiply]

in a `point attribte` SOP:

```c
// create random axis and angle
float angle = rand(@ptnum + chf('seed')) * radians(ch('angle');
vector axis =  rand(@ptnum * chv('axis'));

// create quaternion to rotate by
vector4 extrarot = quaternion(angle, axis);

// rotate existing orient attribute
@orient = qmultiply(@orient, extrarot);
```
