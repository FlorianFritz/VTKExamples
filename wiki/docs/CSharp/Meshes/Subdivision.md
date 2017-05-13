[VTKExamples](/index/)/[CSharp](/CSharp)/Meshes/Subdivision

<img align="right" src="https://github.com/lorensen/VTKExamples/blob/gh-pages/Testing/Baseline/Meshes/TestSubdivision.png?raw=true" width="256" />

### Description
<p>In this example a mesh is read from a file and then subdivided using linear subdivision. The SetNumberOfSubdivisions(n) function controls how many times the mesh is subdivided. For each n, the number of triangles will increase by approximately a factor of 4. For example, if n=2, the number of triangles in the resulting mesh will be 16x the number of triangles in the original mesh.<br />

Different types of subdivisions can be obtained by replacing vtkLinearSubdivisionFilter with either vtkLoopSubdivisionFilter or vtkButterflySubdivisionFilter.</p>A tutorial on how to setup a Windows Forms Application utilizing ActiViz.NET can be found here: [Setup a Windows Forms Application to use ActiViz.NET](http://www.vtk.org/Wiki/VTK/CSharp/ActiViz.NET)<br />
Note: As long as ActiViz.NET is not build with VTK version 6.0 or higher you must define the preprocessor directive VTK_MAJOR_VERSION_5.

**Subdivision.cs**
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
            Subdivision(null); // you may provide a full path to a *.vtu file
         }
         catch(Exception ex) {
            MessageBox.Show(ex.Message, "Exception", MessageBoxButtons.OK);
         }
      }


      private void Subdivision(string filePath) {
         vtkPolyData originalMesh;
         if(filePath != null) {
            vtkXMLPolyDataReader reader = vtkXMLPolyDataReader.New();
            reader.SetFileName(filePath);
            // Subdivision filters only work on triangles
            vtkTriangleFilter triangles = vtkTriangleFilter.New();
            triangles.SetInputConnection(reader.GetOutputPort());
            triangles.Update();
            originalMesh = triangles.GetOutput();
         }
         else {
            vtkSphereSource sphereSource = vtkSphereSource.New();
            sphereSource.Update();
            originalMesh = sphereSource.GetOutput();
         }
         Debug.WriteLine("Before subdivision");
         Debug.WriteLine("    There are " + originalMesh.GetNumberOfPoints()
            + " points.");
         Debug.WriteLine("    There are " + originalMesh.GetNumberOfPolys()
            + " triangles.");

         int numberOfViewports = 3;

         vtkRenderWindow renderWindow = renderWindowControl1.RenderWindow;
         this.Size = new System.Drawing.Size(200 * numberOfViewports + 12, 252);
         this.Text += " - Subdivision";
         Random rnd = new Random(2);
         int numberOfSubdivisions = 2;

         // Create one text property for all
         vtkTextProperty textProperty = vtkTextProperty.New();
         textProperty.SetFontSize(14);
         textProperty.SetJustificationToCentered();

         for(int i = 0; i < numberOfViewports; i++) {
            // Note: Here we create a superclass pointer (vtkPolyDataAlgorithm) so that we can easily instantiate different
            // types of subdivision filters. Typically you would not want to do this, but rather create the pointer to be the type
            // filter you will actually use, e.g. 
            // <vtkLinearSubdivisionFilter>  subdivisionFilter = <vtkLinearSubdivisionFilter>.New();
            vtkPolyDataAlgorithm subdivisionFilter;
            switch(i) {
               case 0:
                  subdivisionFilter = vtkLinearSubdivisionFilter.New();
                  ( (vtkLinearSubdivisionFilter)subdivisionFilter ).SetNumberOfSubdivisions(numberOfSubdivisions);
                  break;
               case 1:
                  subdivisionFilter = vtkLoopSubdivisionFilter.New();
                  ( (vtkLoopSubdivisionFilter)subdivisionFilter ).SetNumberOfSubdivisions(numberOfSubdivisions);
                  break;
               case 2:
                  subdivisionFilter = vtkButterflySubdivisionFilter.New();
                  ( (vtkButterflySubdivisionFilter)subdivisionFilter ).SetNumberOfSubdivisions(numberOfSubdivisions);
                  break;
               default:
                  subdivisionFilter = vtkLinearSubdivisionFilter.New();
                  ( (vtkLinearSubdivisionFilter)subdivisionFilter ).SetNumberOfSubdivisions(numberOfSubdivisions);
                  break;
            }
#if VTK_MAJOR_VERSION_5
            subdivisionFilter.SetInputConnection(originalMesh.GetProducerPort());
#else
            subdivisionFilter.SetInputData(originalMesh);
#endif
            subdivisionFilter.Update();
            vtkRenderer renderer = vtkRenderer.New();
            renderWindow.AddRenderer(renderer);
            renderer.SetViewport((float)i / numberOfViewports, 0, (float)( i + 1 ) / numberOfViewports, 1);
            renderer.SetBackground(.2 + rnd.NextDouble() / 8, .3 + rnd.NextDouble() / 8, .4 + rnd.NextDouble() / 8);

            vtkTextMapper textMapper = vtkTextMapper.New();
            vtkActor2D textActor = vtkActor2D.New();
            textMapper.SetInput(subdivisionFilter.GetClassName());
            textMapper.SetTextProperty(textProperty);

            textActor.SetMapper(textMapper);
            textActor.SetPosition(100, 16);

            //Create a mapper and actor
            vtkPolyDataMapper mapper = vtkPolyDataMapper.New();
            mapper.SetInputConnection(subdivisionFilter.GetOutputPort());
            vtkActor actor = vtkActor.New();
            actor.SetMapper(mapper);
            renderer.AddActor(actor);
            renderer.AddActor(textActor);
            renderer.ResetCamera();
         }
         renderWindow.Render();
      }
   }
}
```
