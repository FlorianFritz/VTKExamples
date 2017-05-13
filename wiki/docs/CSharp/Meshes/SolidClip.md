[VTKExamples](/index/)/[CSharp](/CSharp)/Meshes/SolidClip

<img align="right" src="https://github.com/lorensen/VTKExamples/blob/gh-pages/Testing/Baseline/Meshes/TestSolidClip.png?raw=true" width="256" />

### Description
<p>This example clips a mesh and applies a backface property to that mesh that appears to have a solid interior.</p>
A tutorial on how to setup a Windows Forms Application utilizing ActiViz.NET can be found here: [Setup a Windows Forms Application to use ActiViz.NET](http://www.vtk.org/Wiki/VTK/CSharp/ActiViz.NET)<br />
Note: As long as ActiViz.NET is not build with VTK version 6.0 or higher you must define the preprocessor directive VTK_MAJOR_VERSION_5.

**SolidClip.cs**
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
            SolidClip();
         }
         catch(Exception ex) {
            MessageBox.Show(ex.Message, "Exception", MessageBoxButtons.OK);
         }
      }


      private void SolidClip() {
         // Create a superquadric
         vtkSuperquadricSource superquadricSource = vtkSuperquadricSource.New();
         superquadricSource.SetPhiRoundness(3.1);
         superquadricSource.SetThetaRoundness(2.2);

         // Define a clipping plane
         vtkPlane clipPlane = vtkPlane.New();
         clipPlane.SetNormal(1.0, -1.0, -1.0);
         clipPlane.SetOrigin(0.0, 0.0, 0.0);

         // Clip the source with the plane
         vtkClipPolyData clipper = vtkClipPolyData.New();
#if VTK_MAJOR_VERSION_5
         clipper.SetInputConnection(superquadricSource.GetOutputPort());
#else
         clipper.SetInputData(superquadricSource);
#endif
         clipper.SetClipFunction(clipPlane);

         //Create a mapper and actor
         vtkPolyDataMapper superquadricMapper = vtkPolyDataMapper.New();
#if VTK_MAJOR_VERSION_5
         superquadricMapper.SetInputConnection(clipper.GetOutputPort());
#else
         superquadricMapper.SetInputData(clipper);
#endif
         vtkActor superquadricActor = vtkActor.New();
         superquadricActor.SetMapper(superquadricMapper);

         // Create a property to be used for the back faces. Turn off all
         // shading by specifying 0 weights for specular and diffuse. Max the
         // ambient.
         vtkProperty backFaces = vtkProperty.New();
         backFaces.SetSpecular(0.0);
         backFaces.SetDiffuse(0.0);
         backFaces.SetAmbient(1.0);
         backFaces.SetAmbientColor(1.0000, 0.3882, 0.2784);

         superquadricActor.SetBackfaceProperty(backFaces);
         // get a reference to the renderwindow of our renderWindowControl1
         vtkRenderWindow renderWindow = renderWindowControl1.RenderWindow;
         // renderer
         vtkRenderer renderer = renderWindow.GetRenderers().GetFirstRenderer();
         // add our actor to the renderer
         renderer.AddActor(superquadricActor);
      }
   }
}
```
