[VTKExamples](/home/)/[CSharp](/CSharp)/Meshes/Triangulate

<img align="right" src="https://github.com/lorensen/VTKExamples/blob/gh-pages/Testing/Baseline/Meshes/TestTriangulate.png?raw=true" width="256" />

### Description
A tutorial on how to setup a Windows Forms Application utilizing ActiViz.NET can be found here: [Setup a Windows Forms Application to use ActiViz.NET](http://www.vtk.org/Wiki/VTK/CSharp/ActiViz.NET)<br />
Note: As long as ActiViz.NET is not build with VTK version 6.0 or higher you must define the preprocessor directive VTK_MAJOR_VERSION_5.

**Triangulate.cs**
```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Windows.Forms;
using System.Diagnostics;

using Kitware.VTK;

namespace ActiViz.Examples {
   public partial class Form1 : Form {
      public Form1() {
         InitializeComponent();
      }


      private void renderWindowControl1_Load(object sender, EventArgs e) {
         try {
            Triangulate();
         }
         catch(Exception ex) {
            MessageBox.Show(ex.Message, "Exception", MessageBoxButtons.OK);
         }
      }


      private void Triangulate() {
         vtkRegularPolygonSource polygonSource = vtkRegularPolygonSource.New();
         polygonSource.Update();

         vtkTriangleFilter triangleFilter = vtkTriangleFilter.New();
#if VTK_MAJOR_VERSION_5
         triangleFilter.SetInputConnection(polygonSource.GetOutputPort());
#else
         triangleFilter.SetInputData(polygonSource);
#endif
         triangleFilter.Update();

         vtkPolyDataMapper inputMapper = vtkPolyDataMapper.New();
#if VTK_MAJOR_VERSION_5
         inputMapper.SetInputConnection(polygonSource.GetOutputPort());
#else
         inputMapper.SetInputData(polygonSource);
#endif
         vtkActor inputActor = vtkActor.New();
         inputActor.SetMapper(inputMapper);
         inputActor.GetProperty().SetRepresentationToWireframe();

         vtkPolyDataMapper triangleMapper = vtkPolyDataMapper.New();
#if VTK_MAJOR_VERSION_5
         triangleMapper.SetInputConnection(triangleFilter.GetOutputPort());
#else
         triangleMapper.SetInputData(triangleFilter);
#endif
         vtkActor triangleActor = vtkActor.New();
         triangleActor.SetMapper(triangleMapper);
         triangleActor.GetProperty().SetRepresentationToWireframe();

         vtkRenderWindow renderWindow = renderWindowControl1.RenderWindow;
         this.Size = new System.Drawing.Size(612, 352);

         // Define viewport ranges
         // (xmin, ymin, xmax, ymax)
         double[] leftViewport = new double[] { 0.0, 0.0, 0.5, 1.0 };
         double[] rightViewport = new double[] { 0.5, 0.0, 1.0, 1.0 };

         // Setup both renderers
         vtkRenderer leftRenderer = vtkRenderer.New();
         renderWindow.AddRenderer(leftRenderer);
         leftRenderer.SetViewport(leftViewport[0], leftViewport[1], leftViewport[2], leftViewport[3]);
         leftRenderer.SetBackground(.6, .5, .4);

         vtkRenderer rightRenderer = vtkRenderer.New();
         renderWindow.AddRenderer(rightRenderer);
         rightRenderer.SetViewport(rightViewport[0], rightViewport[1], rightViewport[2], rightViewport[3]);
         rightRenderer.SetBackground(.4, .5, .6);

         // Add the sphere to the left and the cube to the right
         leftRenderer.AddActor(inputActor);
         rightRenderer.AddActor(triangleActor);
         leftRenderer.ResetCamera();
         rightRenderer.ResetCamera();
         renderWindow.Render();
      }
   }
}
```
