---
title: "Houdini Python"
categories:
  - blog
tags:
  - houdini
  - python
---

# Python
Print a list of all materials attributes assigned on a selected node. Made this as I wanted a list to be able to pick materials to override in a redshift proxy!

```c
for n in hou.selectedNodes():
    g = n.geometry()
    a = g.findPrimAttrib('shop_materialpath')
    
    materials = a.strings()
     
    for m in materials:
        print(m)
 ```
 
Check if in UI mode, ie. disable popups or enable things only when rendering
```c
hou.isUIAvailable()
```

Create `null` objects from `transform` SOPs. This can be useful to re-create transforms at object level instead of ‘deforming’ geometry which is much heavier.
```c
def nulls_from_x():
    for node in hou.selectedNodes():
        print(node.name())
        t = node.parmTuple('t').eval()
        r = node.parmTuple('r').eval()
       
        s = node.parmTuple('s').eval()
        scale = node.parm('scale').eval()

        new_null = hou.node('obj/').createNode('null')
        new_null.parmTuple('t').set(t)
        new_null.parmTuple('r').set(r)
        new_null.parmTuple('s').set(s)
        new_null.parm('scale').set(scale)
```
