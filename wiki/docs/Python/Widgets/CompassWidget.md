[VTKExamples](Home)/[Python](Python)/Widgets/CompassWidget

### Description
[]([File:VTK_Examples_Python_Widgets_CompassWidget.png])

Draws an interactive compass.

* Contributed by Jothy

==Build Requirements==
VTK_USE_GEOVIS:BOOL=ON (For VTK < VTK 5.4).

**CompassWidget.py**
```python
#!/usr/bin/env python

import vtk

# sphere
sphereSource = vtk.vtkSphereSource()
sphereSource.SetCenter(0.0, 0.0, 0.0)
sphereSource.SetRadius(4.0)
 
mapper = vtk.vtkPolyDataMapper()
if vtk.VTK_MAJOR_VERSION <= 5:
    mapper.SetInput(sphereSource.GetOutput())
else:
    mapper.SetInputConnection(sphereSource.GetOutputPort())
 
actor = vtk.vtkActor()
actor.SetMapper(mapper)
 
# a renderer and render window
renderer = vtk.vtkRenderer()
renderWindow = vtk.vtkRenderWindow()
renderWindow.AddRenderer(renderer)
 
# an interactor
renderWindowInteractor = vtk.vtkRenderWindowInteractor()
renderWindowInteractor.SetRenderWindow(renderWindow)
 
# Create the widget and its representation
compassRepresentation = vtk.vtkCompassRepresentation()
 
compassWidget = vtk.vtkCompassWidget()
compassWidget.SetInteractor(renderWindowInteractor)
compassWidget.SetRepresentation(compassRepresentation)
 
# add the actors to the scene
renderer.AddActor(actor)
renderer.SetBackground(1, 1, 1) # Background color white
renderWindow.Render()
compassWidget.EnabledOn()
 
style = vtk.vtkInteractorStyleTrackballCamera()
renderWindowInteractor.SetInteractorStyle(style)
 
# begin interaction
renderWindowInteractor.Start()
renderWindowInteractor.Initialize()
```
