[VTKExamples](/home/)/[Python](/Python)//InteractorStyleTrackballActor

### Description
Move, rotate, and scale an object in 3D.

**InteractorStyleTrackballActor.py**
```python
import vtk
 
# create a rendering window and renderer
ren = vtk.vtkRenderer()
renWin = vtk.vtkRenderWindow()
renWin.AddRenderer(ren)
 
# create a renderwindowinteractor
iren = vtk.vtkRenderWindowInteractor()
iren.SetRenderWindow(renWin)
 
style = vtk.vtkInteractorStyleTrackballActor()
iren.SetInteractorStyle(style)
 
# create source
sphereSource = vtk.vtkSphereSource()
sphereSource.Update()
  
# mapper
mapper = vtk.vtkPolyDataMapper()
mapper.SetInput(sphereSource.GetOutput())
 
# actor
actor = vtk.vtkActor()
actor.SetMapper(mapper)

# assign actor to the renderer
ren.AddActor(actor)
 
# enable user interface interactor
iren.Initialize()
renWin.Render()
iren.Start()
```
