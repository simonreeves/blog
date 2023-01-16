---
title: "Houdini Python"
categories:
  - blog
tags:
  - houdini
  - python
---

# Python
## Print a list of all materials attributes assigned on a selected node
Made this as I wanted a list to be able to pick materials to override in a Redshift proxy!

```python
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
## Create `null` objects from `transform` SOPs.

This can be useful to re-create transforms at object level instead of ‘deforming’ geometry which is much heavier.
```python
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

# Shelf/tab tools

## User placement of new nodes in network view

<video autoplay>
  <source src="https://user-images.githubusercontent.com/12150445/212692148-295c0079-8b7e-4454-8b7e-14bc5a3b5cfc.mp4" type="video/mp4">
</video>

  * [Sidefx docs here](https://www.sidefx.com/docs/houdini/hom/tool_script.html)
  * [Production example in my BeeHou module](https://github.com/simonreeves/BeeHou/commit/636ea49891c593e413990cecfd53cf216968df1f)

In the shelf tool, add `kwargs` to the function call:
```python
import bee.focus_null
bee.focus_null.main(kwargs)
```
In the script, instead of using `node.createNode()` which will autoplace a node somewhere possibly unhelpfully:
```python
focus_null = node.parent().createNode('null', node.name() + '_focus')
```
Use `stateutils` and `toolutils` to get the active pane where tab was pressed (check if its a networkeditor) and `toolutils.genericTool` to let the user place the node:
```python
import stateutils
import toolutils

# get network editor pane
pane = stateutils.activePane(kwargs)

if not isinstance(pane, hou.NetworkEditor):
    hou.ui.setStatusMessage('Only works in network editor', severity=hou.severityType.Error)
    return False

# create null      
# toolutils not soptoolutils, because it's not a SOP
focus_null = toolutils.genericTool(kwargs, 'null')
```


