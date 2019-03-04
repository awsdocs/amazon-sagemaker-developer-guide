# Build a Model<a name="build-model"></a>

To build a machine learning model that can be used for making accurate inferences, three steps are required:
+ **Select the mode**: You need to choose machine learning model that is appropriate for the type and quantity of data you have, or can collect, and for the problem that you are solving\. You can use a built\-in algorithm provided by Amazon SageMaker, your own custom algorithm, one of the deep learning frameworks supported by Amazon SageMaker, or an algorithm that you subscribe to from AWS Marketplace\. Options are outlined in [Choose a Machine Learning Algorithm](#select-ml-algorithms)\. 
+ **Train the model**: This can be done in a variety of ways that depend on the type and size of the training data and on the algorithms to be used\. Options are outlined in the [Train a Model](train-model.md) section\.
+ **Validate the model**: The accuracy of the trained model must be assessed against an additional validation dataset\. Some guidance on approaching this step is provided in the [Validate a Machine Learning Model](how-it-works-model-validation.md) topic\.

**Topics**
+ [Choose a Machine Learning Algorithm](#select-ml-algorithms)
+ [Use Amazon SageMaker Built\-in Algorithms](algos.md)
+ [Use Your Own Algorithms with Amazon SageMaker](your-algorithms.md)

## Choose a Machine Learning Algorithm<a name="select-ml-algorithms"></a>

This section focuses on the first step in building your model: selecting the appropriate machine learning model\. This depends on the type of predictions that you want to make and on the type and amount of data available to you\.
+ For guidance on using a built\-in algorithm provided by Amazon SageMaker, see [Use Amazon SageMaker Built\-in Algorithms ](algos.md)\.
+ For guidance on using your own custom algorithm, see [Use Your Own Algorithms with Amazon SageMaker](your-algorithms.md)\. 
+ For guidance on using one of the deep leaning frameworks supported by Amazon SageMaker, see [Use Machine Learning Frameworks with Amazon SageMaker](frameworks.md)\.
+ For guidance on using an algorithm that you subscribe to from AWS Marketplace, see [Amazon SageMaker Resources in AWS Marketplace](sagemaker-marketplace.md)\.