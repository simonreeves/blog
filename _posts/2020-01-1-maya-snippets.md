---
title: "Maya Snippets"
categories:
  - Maya
tags:
  - maya
  - expressions
  - python
---


# Maya Snippets
I try to use pymel whenever possible, basically a wrapper for cmds, which is for mel... but it object oriented ♥


## Expressions

expressions are their own DAG object, written like `object.attr = frame * 2`
```
PNT_Prop_RecordPlayer_Arm001.rotateY = (noise(time*10)*.1)+12
```
can be made in python simply with a string
```python
# example from a script with a blendshape
expr_string = '{obj}.{attr} = frame) * (1.0/5.0)'.format(obj=bs, attr='polyShape1')
expr = cmds.expression(string=expr_string)

```
Alembic export, disable viewports, add timer
```python
import time
from maya import mel

ExportPath = 'M:/path/to/mycache_v014.abc'

TimerStart = time.time()

mel.eval("paneLayout -e -manage false $gMainPane")
mel.eval('AbcExport -j "-frameRange 991 1100 -dataFormat ogawa -file {}"'.format(ExportPath))
mel.eval("paneLayout -e -manage true $gMainPane")

TimerEnd = time.time()
print('TimeTaken: {0}s'.format(round(TimerEnd - TimerStart, 2)))
```

## Maya Stuff
```python
# Check maya version
import pymel.versions
if pymel.versions.current() >= pymel.versions.v2008:
    print("The current version is later than Maya 2008")

# returns something like 201400
print(pymel.versions.current())
```

## Object stuff
```python
import pymel.core as pm

# returns objects (not strings like cmds)
selection = pm.selected()

for selected in selection:
    # check if selection is a transform (group or object)
    print(selected.getShape())
            
    if isinstance(selected, pm.nodetypes.Transform):
        # Check if first child is a shape? mesh
        print(type(selected.childAtIndex(0)))

        if isinstance(selected.childAtIndex(0), pm.nodetypes.Mesh):
            print('{} is a mesh really'.format(selected))
        else:
            print('{} is a transform probably'.format(selected))

    # name
    print(selected.name())

    # full name!
    print(selected.fullName())
    
    # pymel set attr on an object
    selection.setAttr('displayResolution', 1)
    selection.setAttr('displayGateMaskColor', (0.0, 0.0, 0.0))
    selection.setAttr('overscan', 1.0)

    # node type
    print(node.type())

    # Get namespace (returns something like Car: so also remove the seperator)
    selection.namespace()[:-1]


# pymel if you have the attribute just use set()
attribute.set('lalala')

```

## Scene

```python
# Get Scene name
scene_name = pm.sceneName()

# Get all renderlayers
for Node in pm.ls(type='renderLayer'):
    print(Node)
    
# Get all vray elememts
for Node in pm.ls(type='VRayRenderElement'):
    print(Node)


# Set timeline
frame_start = 1186
frame_end = 1208

pm.playbackOptions(
    ast=frame_start,
    min=frame_start,
    aet=frame_end,
    max=frame_end
)

# Set render range
pm.setAttr('defaultRenderGlobals.startFrame', FrameStart )
pm.setAttr('defaultRenderGlobals.endFrame', FrameEnd)

# get timeline range - there is no class, just a function
frame_start = pm.playbackOptions(query=True, min=True)
frame_end = pm.playbackOptions(query=True, max=True)
```

# cmds boo

```python
# Get an existing node by name
vray_settings_node = pm.ls(type='VRaySettingsNode')

# vray settings!
cmds.createNode('VRaySettingsNode', name='vraySettings')

# cmds set an attribute on a node (given as string)
cmds.setAttr('vraySettings.fileNamePrefix', render_path, type='string')
cmds.setAttr('defaultRenderGlobals.ren', 'vray', type='string')

# Get the node type
print(cmds.nodeType('vraySettings'))

# Get selected node type
print(cmds.nodeType(cmds.ls(selection=True)))

# Get/Set attributes vraymaterial is the node
print(VRayMaterial.vrayMaterialId.get())
VRayMaterial.vrayMaterialId.set(55)

# List all the atrriubtes on a node
for attribute in cmds.listAttr('vraySettings'):
    print(attribute)

# pymel
for attribute in selection[0].listAttr():
    print(attribute)

# Find out the current renderer
print('Current Renderer: ' + str(cmds.getAttr('defaultRenderGlobals.)currentRenderer'))

# Set renderer
cmds.setAttr('defaultRenderGlobals.ren', 'vray', type='string')

# Get Scene name
# Via CMDs
ScenePath = cmds.file(q=True, sceneName=True)


# Current selection
selection = cmds.ls(selection=True)
  

# Unlock and flip normals, based on selection, no option to be specific...
cmds.polyNormalPerVertex(unFreezeNormal=True)
cmds.ReversePolygonNormals()


# Get render range
print(cmds.getAttr('defaultRenderGlobals.startFrame'))
print(cmds.getAttr('defaultRenderGlobals.endFrame'))
    





############################### Adding a menu ###############################

def AddAnalogMenu():
    
    # How to find the main menu command
    # MainWindow = pm.getMelGlobal('string', 'gMainWindow')
    # this returns as 'MayaWindow' which is what we want to use

    # Make a menu, give it a parent and a label
    AnalogMenu = pm.menu( label='Analog', parent = 'MayaWindow')
    
    # Add menu items, with functions to execute when called
    pm.menuItem( parent = AnalogMenu, label='Analog Save', command = AnalogSave_Execute )
    pm.menuItem( parent = AnalogMenu, label='Deadline', command = deadline_Execute )

    
# Wait until maya is idle, everything has loaded, then run custom stuff
cmds.evalDeferred(AddAnalogMenu)


```
