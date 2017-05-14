[VTKExamples](/index/)/[Cxx](/Cxx)/GeometricObjects/Quad

<img align="right" src="https://github.com/lorensen/VTKExamples/blob/gh-pages/Testing/Baseline/GeometricObjects/TestQuad.png?raw=true" width="256" />

**Quad.cxx**
```c++
#include <vtkVersion.h>
#include <vtkCellArray.h>
#include <vtkPoints.h>
#include <vtkQuad.h>
#include <vtkPolyData.h>
#include <vtkSmartPointer.h>
#include <vtkPolyDataMapper.h>
#include <vtkActor.h>
#include <vtkRenderWindow.h>
#include <vtkRenderer.h>
#include <vtkRenderWindowInteractor.h>

int main(int , char *[])
{
  // Create four points (must be in counter clockwise order)
  double p0[3] = {0.0, 0.0, 0.0};
  double p1[3] = {1.0, 0.0, 0.0};
  double p2[3] = {1.0, 1.0, 0.0};
  double p3[3] = {0.0, 1.0, 0.0};
      
  // Add the points to a vtkPoints object
  vtkSmartPointer<vtkPoints> points =
    vtkSmartPointer<vtkPoints>::New();
  points->InsertNextPoint(p0);
  points->InsertNextPoint(p1);
  points->InsertNextPoint(p2);
  points->InsertNextPoint(p3);
  
  // Create a quad on the four points
  vtkSmartPointer<vtkQuad> quad =
    vtkSmartPointer<vtkQuad>::New();
  quad->GetPointIds()->SetId(0,0);
  quad->GetPointIds()->SetId(1,1);
  quad->GetPointIds()->SetId(2,2);
  quad->GetPointIds()->SetId(3,3);
  
  // Create a cell array to store the quad in
  vtkSmartPointer<vtkCellArray> quads =
    vtkSmartPointer<vtkCellArray>::New();
  quads->InsertNextCell(quad);

  // Create a polydata to store everything in
  vtkSmartPointer<vtkPolyData> polydata =
    vtkSmartPointer<vtkPolyData>::New();

  // Add the points and quads to the dataset
  polydata->SetPoints(points);
  polydata->SetPolys(quads);

  // Setup actor and mapper
  vtkSmartPointer<vtkPolyDataMapper> mapper =
    vtkSmartPointer<vtkPolyDataMapper>::New();
#if VTK_MAJOR_VERSION <= 5
  mapper->SetInput(polydata);
#else
  mapper->SetInputData(polydata);
#endif
  
  vtkSmartPointer<vtkActor> actor =
    vtkSmartPointer<vtkActor>::New();
  actor->SetMapper(mapper);
  
  // Setup render window, renderer, and interactor
  vtkSmartPointer<vtkRenderer> renderer =
    vtkSmartPointer<vtkRenderer>::New();
  vtkSmartPointer<vtkRenderWindow> renderWindow =
    vtkSmartPointer<vtkRenderWindow>::New();
  renderWindow->AddRenderer(renderer);
  vtkSmartPointer<vtkRenderWindowInteractor> renderWindowInteractor = 
    vtkSmartPointer<vtkRenderWindowInteractor>::New();
  renderWindowInteractor->SetRenderWindow(renderWindow);
  renderer->AddActor(actor);
  renderWindow->Render();
  renderWindowInteractor->Start();

  return EXIT_SUCCESS;
}
```
**CMakeLists.txt**
```cmake
cmake_minimum_required(VERSION 2.8)
 
PROJECT(Quad)
 
find_package(VTK REQUIRED)
include(${VTK_USE_FILE})
 
add_executable(Quad MACOSX_BUNDLE Quad.cxx)
 
target_link_libraries(Quad ${VTK_LIBRARIES})
```

**Download and Build Quad**

Click [here to download Quad](https://github.com/lorensen/VTKWikiExamplesTarballs/raw/master/Quad.tar) and its *CMakeLists.txt* file.
Once the *tarball Quad.tar* has been downloaded and extracted,
```
cd Quad/build 
```
If VTK is installed:
```
cmake ..
```
If VTK is not installed but compiled on your system, you will need to specify the path to your VTK build:
```
cmake -DVTK_DIR:PATH=/home/me/vtk_build ..
```
Build the project:
```
make
```
and run it:
```
./Quad
```
**WINDOWS USERS PLEASE NOTE:** Be sure to add the VTK bin directory to your path. This will resolve the VTK dll's at run time.

