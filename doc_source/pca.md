# Principal Component Analysis \(PCA\) Algorithm<a name="pca"></a>

PCA is an unsupervised machine learning algorithm that attempts to reduce the dimensionality \(number of features\) within a dataset while still retaining as much information as possible\. This is done by finding a new set of features called *components*, which are composites of the original features that are uncorrelated with one another\. They are also constrained so that the first component accounts for the largest possible variability in the data, the second component the second most variability, and so on\.

In Amazon SageMaker, PCA operates in two modes, depending on the scenario: 
+ **regular**: For datasets with sparse data and a moderate number of observations and features\.
+ **randomized**: For datasets with both a large number of observations and features\. This mode uses an approximation algorithm\. 

PCA uses tabular data\. 

The rows represent observations you want to embed in a lower dimensional space\. The columns represent features that you want to find a reduced approximation for\. The algorithm calculates the covariance matrix \(or an approximation thereof in a distributed manner\), and then performs the singular value decomposition on this summary to produce the principal components\. 

**Topics**
+ [Input/Output Interface for the PCA Algorithm](#pca-inputoutput)
+ [EC2 Instance Recommendation for the PCA Algorithm](#pca-instances)
+ [PCA Sample Notebooks](#PCA-sample-notebooks)
+ [How PCA Works](how-pca-works.md)
+ [PCA Hyperparameters](PCA-reference.md)
+ [PCA Response Formats](PCA-in-formats.md)

## Input/Output Interface for the PCA Algorithm<a name="pca-inputoutput"></a>

For training, PCA expects data provided in the train channel, and optionally supports a dataset passed to the test dataset, which is scored by the final algorithm\. Both `recordIO-wrapped-protobuf` and `CSV` formats are supported for training\. You can use either File mode or Pipe mode to train models on data that is formatted as `recordIO-wrapped-protobuf` or as `CSV`\.

For inference, PCA supports `text/csv`, `application/json`, and `application/x-recordio-protobuf`\. Results are returned in either `application/json` or `application/x-recordio-protobuf` format with a vector of "projections\."

For more details on training and inference file formats, see the [PCA Sample Notebooks](#PCA-sample-notebooks) and the [PCA Response Formats](PCA-in-formats.md)\.

For more information on input and output file formats, see [PCA Response Formats](PCA-in-formats.md) for inference and the [PCA Sample Notebooks](#PCA-sample-notebooks)\.

## EC2 Instance Recommendation for the PCA Algorithm<a name="pca-instances"></a>

PCA supports both GPU and CPU computation\. Which instance type is most performant depends heavily on the specifics of the input data\.

## PCA Sample Notebooks<a name="PCA-sample-notebooks"></a>

For a sample notebook that shows how to use the SageMaker Principal Component Analysis algorithm to analyze the images of handwritten digits from zero to nine in the MNIST dataset, see [An Introduction to PCA with MNIST](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/pca_mnist/pca_mnist.ipynb)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. Once you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the SageMaker samples\. The topic modeling example notebooks using the NTM algorithms are located in the **Introduction to Amazon algorithms** section\. To open a notebook, click on its **Use** tab and select **Create copy**\.