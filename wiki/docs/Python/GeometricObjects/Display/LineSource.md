[VTKExamples](/index/)/[Python](/Python)/GeometricObjects/Display/LineSource

**LineSource.py**
```python
import vtk

# create a rendering window and renderer
ren = vtk.vtkRenderer()
renWin = vtk.vtkRenderWindow()
renWin.AddRenderer(ren)

# create a renderwindowinteractor
iren = vtk.vtkRenderWindowInteractor()
iren.SetRenderWindow(renWin)


# create source
source = vtk.vtkLineSource()
source.SetPoint1(1,-1,0)
source.SetPoint2(2,-3,0)

# mapper
mapper = vtk.vtkPolyDataMapper()
mapper.SetInput(source.GetOutput())

# actor
actor = vtk.vtkActor()
actor.SetMapper(mapper)

# color actor
actor.GetProperty().SetColor(1,0,1)

# assign actor to the renderer
ren.AddActor(actor)

# enable user interface interactor
iren.Initialize()
renWin.Render()
iren.Start()
```
