# Using Apache Spark with Amazon SageMaker<a name="apache-spark"></a>

This section provides information for developers who want to use Apache Spark for preprocessing data and Amazon SageMaker for model training and hosting\. For information about supported versions of Apache Spark, see [Supported Versions](supported-versions.md)\. 

Amazon SageMaker provides an Apache Spark library, in both Python and Scala, that you can use to easily train models in Amazon SageMaker using `org.apache.spark.sql.DataFrame`s in your Spark clusters\. After model training, you can also host the model using Amazon SageMaker hosting services\. 

The Amazon SageMaker Spark library, `com.amazonaws.services.sagemaker.sparksdk`, provides the following classes, among others:

+ `SageMakerEstimator`—Extends the `org.apache.spark.ml.Estimator` interface\. You can use this estimator for model training in Amazon SageMaker\.

+ `KMeansSageMakerEstimator`, `PCASageMakerEstimator`, and `XGBoostSageMakerEstimator`—Extend the `SageMakerEstimator` class\. 

+ `SageMakerModel`—Extends the `org.apache.spark.ml.Model` class\. You can use this `SageMakerModel` for model hosting and obtaining inferences in Amazon SageMaker\.

## Downloading the Amazon SageMaker Spark Library<a name="spark-sdk-download"></a>

You have the following options for downloading the Spark library provided by Amazon SageMaker:

+ You can download the source code for both PySpark and Scala libraries from GitHub at [https://github\.com/aws/sagemaker\-spark](https://github.com/aws/sagemaker-spark)\. 

   

+ For the Python Spark library, you have the following additional options:

  + Use pip install:

    ```
    $ pip install sagemaker_pyspark
    ```

  + If you are using a Jupyter notebook on your Amazon SageMaker notebook instance to develop applications, the PySpark library is also available on your notebook instance\. 

     

+ You can get the Scala library from Maven\. Add the Spark library to your project by adding the following dependency to your pom\.xml file:

  ```
  <dependency>
      <groupId>com.amazonaws</groupId>
      <artifactId>sagemaker-spark_2.11</artifactId>
      <version>spark_2.2.0-1.0</version>
  </dependency>
  ```

## Integrating Your Apache Spark Application with Amazon SageMaker<a name="spark-sdk-common-process"></a>

The following is high\-level summary of the steps for integrating your Apache Spark application with Amazon SageMaker\.

1. Continue data preprocessing using the Apache Spark library that you are familiar with\. Your dataset remains a `DataFrame` in your Spark cluster\. 
**Note**  
Load your data into a `DataFrame` and preprocess it so that you have a `features` column with `org.apache.spark.ml.linalg.Vector` of `Doubles`, and an optional `label` column with values of `Double`​ type\.

1. Use the estimator in the Amazon SageMaker Spark library to train your model\. For example, if you choose the k\-means algorithm provided by Amazon SageMaker for model training, you call the `KMeansSageMakerEstimator.fit` method\. 

   Provide your `DataFrame` as input\. The estimator returns a `SageMakerModel` object\. 
**Note**  
`SageMakerModel` extends the `org.apache.spark.ml.Model`\.

   The `fit` method does the following: 

   1. Converts the input `DataFrame` to the protobuf format by selecting the `features` and `label` columns from the input `DataFrame` and uploading the protobuf data to an Amazon S3 bucket\. The protobuf format is efficient for model training in Amazon SageMaker\.

   1. Starts model training in Amazon SageMaker by sending an Amazon SageMaker [CreateTrainingJob](API_CreateTrainingJob.md) request\. After model training has completed, Amazon SageMaker saves the model artifacts to an S3 bucket\. 

      Amazon SageMaker assumes the IAM role that you specified for model training to perform tasks on your behalf, for example, to read training data from an S3 bucket and to write model artifacts to an S3 bucket\. 

   1. Creates and returns a `SageMakerModel` object\. The constructor does the following tasks, which are related to deploying your model to Amazon SageMaker\. 

      1. Sends a [CreateModel](API_CreateModel.md) request to Amazon SageMaker\. 

      1. Sends a [CreateEndpointConfig](API_CreateEndpointConfig.md) request to Amazon SageMaker\.

      1. Sends a [CreateEndpoint](API_CreateEndpoint.md) request to Amazon SageMaker, which then launches the specified resources, and hosts the model on them\. 

1. You can get inferences from your model hosted in Amazon SageMaker with the `SageMakerModel.transform`\. 

   Provide an input `DataFrame` with features as input\. The `transform` method transforms it to a `DataFrame` containing inferences\. Internally, the `transform` methods sends a request to the [InvokeEndpoint](API_runtime_InvokeEndpoint.md) Amazon SageMaker API to get inferences\. The `transform` method appends the inferences to the input `DataFrame`\.