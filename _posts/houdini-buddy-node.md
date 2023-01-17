---
title: "Houdini Buddy Node"
tags:
  - houdini
  - python
  - networkeditor
---

This is WIP while I work on it


```python
# eg. (nodes drag&dropped into pythonshell)
m = hou.node('/obj/action/drawers/material_drawers1')
n = hou.node('/obj/action/drawers/matnet1')

# create move event
def move_matnet(event_type, **kwargs):
    # set matnet 4 units to the right of material node
    n.setPosition(m.position() + hou.Vector2((4,0.0)))

# add eventcallback
m.addEventCallback((hou.nodeEventType.PositionChanged,), move_matnet)
```
![node_buddy](https://user-images.githubusercontent.com/12150445/212907655-5dbf3e1a-7e7c-4007-87b3-2e87d0aa241e.gif)


draw a line, or a connectionline between nodes this only flashes up briefly
```python
# networkeditor (drag&dropped into pythonshell)
nw = hou.ui.findPaneTab('panetab7')
s = hou.NetworkShapeConnection(input_pos=n.position(), input_dir=hou.Vector2(0,1), output_pos=m.position(), output_dir=hou.Vector2(0,1),dashed=True)
nw.setShapes((s,))
```
![setshape](https://user-images.githubusercontent.com/12150445/212908407-017f6205-bedf-4fab-b672-b4cffd3c6c03.gif)
