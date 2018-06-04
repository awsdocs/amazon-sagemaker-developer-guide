# TensorFlow Example 1: Using the tf\.estimator<a name="tf-example1"></a>

This introductory TensorFlow example demonstrates how to use the Amazon SageMaker `sagemaker.tensorflow.TensorFlow` estimator class\. This class provides the `fit` method for model training in Amazon SageMaker and the `deploy` method to deploy the resulting model in Amazon SageMaker\. It is included in the Amazon SageMaker high\-level Python library\. 

TensorFlow provides a high\-level machine learning API \(`tf.estimator`\) to easily configure, train, and evaluate a variety of machine learning models\. In this exercise, you construct a neural network classifier using this API\. You then train it on Amazon SageMaker using the []() [Iris flower data set](https://en.wikipedia.org/wiki/Iris_flower_data_set)\. 

In the example code, you do the following:

1. Train the model\. During training, the following occurs: 

   1. Amazon SageMaker loads the Docker image that contains the TensorFlow framework \(this starts the Docker container\)\.

   1. Amazon SageMaker reads training data from an Amazon S3 bucket into the container's file system\. 

   1. Your custom training code constructs a neural network classifier, `tf.estimator.DNNClassifier`\.

   1. The Amazon SageMaker code in the container runs training\. Your training code reads the training data\.

1. Deploy the model using Amazon SageMaker hosting service\. Amazon SageMaker returns an endpoint that you send requests to get inferences\. 

1. Test the model\. You send requests to the model to classify flower samples you send in the request\.

## About the Training Dataset<a name="tf-script-own-algo-ex1-about-dataset"></a>

The Iris flower training dataset used in this exercise contains 150 rows of data, with 50 samples from each of the three related Iris species \(Iris setosa, Iris virginica, and Iris versicolor\)\. Each row in the dataset contains the following data for each of the flower samples:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/iris-dataset.png)

For this exercise, the dataset is randomized and split into two \.csv files:
+ A training dataset of 120 samples \(`iris_training.csv`\)
+ A test dataset of 30 samples \(`iris_test.csv`\)

### Next Step<a name="tf-example1-nexttopic"></a>

 [Step 1: Create a Notebook and Initialize Variables](tf-example1-prepare.md) 