# Principal Component Analysis \(PCA\)<a name="pca"></a>

PCA is an unsupervised machine learning algorithm that attempts to reduce the dimensionality \(number of features\) within a dataset while still retaining as much information as possible\. This is done by finding a new set of features called *components*, which are composites of the original features that are uncorrelated with one another\. They are also constrained so that the first component accounts for the largest possible variability in the data, the second component the second most variability, and so on\.

In Amazon SageMaker, PCA operates in two modes, depending on the scenario: 
+ **regular**: For datasets with sparse data and a moderate number of observations and features\.
+ **randomized**: For datasets with both a large number of observations and features\. This mode uses an approximation algorithm\. 

PCA uses tabular data\. 

The rows represent observations you want to embed in a lower dimensional space\. The columns represent features that you want to find a reduced approximation for\. The algorithm calculates the covariance matrix \(or an approximation thereof in a distributed manner\), and then performs the singular value decomposition on this summary to produce the principal components\. 

## Input/Output Interface<a name="pca-inputoutput"></a>

PCA expects data provided in the train channel, and optionally supports a dataset passed to the test dataset, which is scored by the final algorithm\. Both `recordIO-wrapped-protobuf` and `CSV` file formats are supported\. PCA can be trained in File or Pipe mode when using recordIO\-wrapped protobuf, but only in File mode for the `CSV` format\.

For inference, PCA supports `text/csv`, `application/json`, and `application/x-recordio-protobuf`\. Results are returned in either `application/json` or `application/x-recordio-protobuf` format with a vector of "projections\."

Please refer to the example notebooks for additional details on training and inference formats\.

## EC2 Instance Recommendation<a name="pca-instances"></a>

PCA supports both GPU and CPU computation\. Which instance type is most performant depends heavily on the specifics of the input data\.

**Topics**
+ [Input/Output Interface](#pca-inputoutput)
+ [EC2 Instance Recommendation](#pca-instances)
+ [How PCA Works](how-pca-works.md)
+ [PCA Hyperparameters](PCA-reference.md)
+ [PCA Response Formats](PCA-in-formats.md)