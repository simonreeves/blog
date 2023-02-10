---
title: "Maya Snippets"
categories:
  - Maya
tags:
  - maya
  - expressions
  - python
---


# Maya Python Snippets

# Expressions

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

ExportPath = 'M:/0131_Shynola_Maserati/D_3D_PRODUCTION/03_SHOTS/Shot_220/02_CACHES/Maserati_Shot_220_ForFX_v014.abc'

TimerStart = time.time()

mel.eval("paneLayout -e -manage false $gMainPane")
mel.eval('AbcExport -j "-frameRange 991 1100 -dataFormat ogawa -file {}"'.format(ExportPath))
mel.eval("paneLayout -e -manage true $gMainPane")

TimerEnd = time.time()
print('TimeTaken: {0}s'.format(round(TimerEnd - TimerStart, 2)))
```


```python

# Check maya version
from pymel import versions
if versions.current() >= versions.v2008:
    print("The current version is later than Maya 2008")

# returns something like 201400
print(versions.current())


# To use mel commands via python (basically)
import maya.cmds as cmds

# pymel
import pymel.core as pm

# Messy stuff here
    # Check if selection is an object, ie. a transform with a shape child

    # check if selection is a transform (group or object)
    # a test of type(Selection)
    print Selection[0].getShape()
            
    if isinstance(Selection[0], pm.nodetypes.Transform):
        # Check if first child is a shape? mesh
        # print type(Selection[0].childAtIndex(0))
        if isinstance(Selection[0].childAtIndex(0), pm.nodetypes.Mesh):

            print Selection[0] + ' is a mesh really'
        else:
            print Selection[0] + ' is a group probably'

    print Selection[0].fullName()
    
    
    
# Create a node (everything is a node), providing type and name
cmds.createNode('VRaySettingsNode', name='vraySettings')

# Get an existing node by name
VRaySettingsNode = pm.ls(type='VRaySettingsNode')

# Set an attribute on a node, Node.Attriubte
cmds.setAttr('vraySettings.fileNamePrefix', oRenderDir, type='string')
cmds.setAttr('defaultRenderGlobals.ren', 'vray', type='string')

# pymel
Selection[0].setAttr('displayResolution', 1)
Selection[0].setAttr('displayGateMaskColor', (0.0, 0.0, 0.0))
Selection[0].setAttr('overscan', 1.0)


# Set attribute pymel
Attribute.set('lalala')


# Get the node type
print cmds.nodeType('vraySettings')
# Get selected node type
print cmds.nodeType(cmds.ls(selection=True))


# pymel version is much tidier
# if you have a node
Node.nodeType()

# Just do type!
Node.type()


# Get/Set attributes # vraymaterial is the node
print VRayMaterial.vrayMaterialId.get()
VRayMaterial.vrayMaterialId.set(55)


# List all the atrriubtes on a node
for Attribute in cmds.listAttr('vraySettings'):
    print Attribute

# pymel
for Attribute in Selection[0].listAttr():
    print Attribute



# Find out the current renderer
print 'Current Renderer: ' + str(cmds.getAttr('defaultRenderGlobals.currentRenderer'))

# Set renderer
cmds.setAttr('defaultRenderGlobals.ren', 'vray', type='string')


# Get Scene name
# Via CMDs
ScenePath = cmds.file(q=True, sceneName=True)
# Via PyMel
SceneName = pm.sceneName()



# Get all renderlayers
for Node in pm.ls(type='renderLayer'):
    print Node
    
# Get all vray elememts
for Node in pm.ls(type='VRayRenderElement'):
    print Node

    
    
# Current Selection
Selection = cmds.ls(selection=True)
# pymel
Selection = pm.selected()

# Get namespace (returns something like Car: so also remove the seperator)
Selection[0].namespace()[:-1]

# Unlock and flip normals, based on selection, no option to be specific...
cmds.polyNormalPerVertex(unFreezeNormal=True)
cmds.ReversePolygonNormals()


# Set timeline
FrameStart = 1186
FrameEnd = 1208

pm.playbackOptions(ast=FrameStart, min=FrameStart, aet=FrameEnd, max=FrameEnd )

# Set render range
pm.setAttr('defaultRenderGlobals.startFrame', FrameStart )
pm.setAttr('defaultRenderGlobals.endFrame', FrameEnd)

# Get render range
print cmds.getAttr('defaultRenderGlobals.startFrame')
print cmds.getAttr('defaultRenderGlobals.endFrame')
    
# get timeline range - there is no class, just a function
StartFrame = pm.playbackOptions(query=True, min=True)
EndFrame = pm.playbackOptions(query=True, max=True)




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
