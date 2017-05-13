[VTKExamples](/home/)/[CSharp](/CSharp)/GeometricObjects/Plane

<img align="right" src="https://github.com/lorensen/VTKExamples/blob/gh-pages/Testing/Baseline/GeometricObjects/TestPlane.png?raw=true" width="256" />

### Description
A tutorial on how to setup a Windows Forms Application utilizing ActiViz.NET can be found here: [Setup a Windows Forms Application to use ActiViz.NET](http://www.vtk.org/Wiki/VTK/CSharp/ActiViz.NET)

**Plane.cs**
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
            Plane();
         }
         catch(Exception ex) {
            MessageBox.Show(ex.Message, "Exception", MessageBoxButtons.OK);
         }
      }


      private void Plane() {
         // Create a plane
         vtkPlaneSource planeSource = vtkPlaneSource.New();
         planeSource.SetCenter(1.0, 0.0, 0.0);
         planeSource.SetNormal(1.0, 0.0, 1.0);
         planeSource.Update();

         // Visualize
         vtkPolyDataMapper mapper = vtkPolyDataMapper.New();
         mapper.SetInputConnection(planeSource.GetOutputPort());
         vtkActor actor = vtkActor.New();
         actor.SetMapper(mapper);
         actor.GetProperty().SetLineWidth(4);
         vtkRenderWindow renderWindow = renderWindowControl1.RenderWindow;
         vtkRenderer renderer = renderWindow.GetRenderers().GetFirstRenderer();
         renderer.SetBackground(0.2, 0.3, 0.4);
         renderer.AddActor(actor);
         renderer.ResetCamera();
      }
   }
}
```
