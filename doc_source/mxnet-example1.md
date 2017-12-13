# Apache MXNet Example 1: Using the Module API<a name="mxnet-example1"></a>

This introductory Apache MXNet example demonstrates using Amazon SageMaker `sagemaker.mxnet.MXNet` estimator class, provided as part of Amazon SageMaker high\-level Python library\. It provides the `fit` method for model training in Amazon SageMaker and `deploy` method to deploy resulting model in Amazon SageMaker\. 

In this exercise, you construct a neural network classifier using the Apache MXNet `Module` API\. You then train the model using the []() [The MNIST Database](http://yann.lecun.com/exdb/mnist/) dataset, which Amazon SageMaker provides in an S3 bucket\. 

In this example, you do the following

1. Train the model\. During training, the following occurs:

   1. Amazon SageMaker loads the Docker image containing the Apache MXNet framework\.

   1. Amazon SageMaker reads training data from the S3 bucket into the container's file system\.

   1. Your custom training code constructs a neural network classifier \(using the `mxnet.module.Module` class\)\.

   1. The Amazon SageMaker code in the container runs training\. Your training code reads the training data for model training\.

1. Deploy the model using Amazon SageMaker hosting services\. Amazon SageMaker returns an endpoint that you send requests to to get inferences\.

1. Test the model\. The example provides an HTML canvas in the notebook where you can write a single\-digit number using your mouse\. The image of the number is then sent to the model for inference\. 


+ [Step 1: Create a Notebook and Initialize Variables](mxnet-example1-prepare.md)
+ [Step 2: Train a Model on Amazon SageMaker Using Apache MXNet Custom Code](mxnet-part1-train.md)
+ [Step 3 : Deploy the Trained Model](mxnet-example1-deploy.md)
+ [Step 4 : Invoke the Endpoint to Get Inferences](mxnet-example-invoke.md)