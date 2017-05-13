### Description
<source lang="cpp">
#ifndef __vtkTestGraphAlgorithmFilter_h
#define __vtkTestGraphAlgorithmFilter_h

#include "vtkGraphAlgorithm.h"

class vtkTestGraphAlgorithmFilter : public vtkGraphAlgorithm 
{
public:
  vtkTypeMacro(vtkTestGraphAlgorithmFilter,vtkGraphAlgorithm);
  
  static vtkTestGraphAlgorithmFilter *New();
	
protected:
  vtkTestGraphAlgorithmFilter(){}
  ~vtkTestGraphAlgorithmFilter(){}
  
  int RequestData(vtkInformation *, vtkInformationVector **, vtkInformationVector *);

private:
  vtkTestGraphAlgorithmFilter(const vtkTestGraphAlgorithmFilter&);  // Not implemented.
  void operator=(const vtkTestGraphAlgorithmFilter&);  // Not implemented.

};

#endif
</source>
