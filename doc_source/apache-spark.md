# Use Apache Spark with Amazon SageMaker<a name="apache-spark"></a>

This section provides information for developers who want to use Apache Spark for preprocessing data and Amazon SageMaker for model training and hosting\. For information about supported versions of Apache Spark, see the [Getting SageMaker Spark](https://github.com/aws/sagemaker-spark#getting-sagemaker-spark) page in the SageMaker Spark GitHub repository\.

SageMaker provides an Apache Spark library, in both Python and Scala, that you can use to easily train models in SageMaker using `org.apache.spark.sql.DataFrame` data frames in your Spark clusters\. After model training, you can also host the model using SageMaker hosting services\. 

The SageMaker Spark library, `com.amazonaws.services.sagemaker.sparksdk`, provides the following classes, among others:
+ `SageMakerEstimator`—Extends the `org.apache.spark.ml.Estimator` interface\. You can use this estimator for model training in SageMaker\.
+ `KMeansSageMakerEstimator`, `PCASageMakerEstimator`, and `XGBoostSageMakerEstimator`—Extend the `SageMakerEstimator` class\. 
+ `SageMakerModel`—Extends the `org.apache.spark.ml.Model` class\. You can use this `SageMakerModel` for model hosting and obtaining inferences in SageMaker\.

## Download the SageMaker Spark Library<a name="spark-sdk-download"></a>

You have the following options for downloading the Spark library provided by SageMaker:
+ You can download the source code for both PySpark and Scala libraries from the [SageMaker Spark](https://github.com/aws/sagemaker-spark) GitHub repository\. 
+ For the Python Spark library, you have the following additional options:
  + Use pip install:

    ```
    $ pip install sagemaker_pyspark
    ```
  + In a notebook instance, create a new notebook that uses either the `Sparkmagic (PySpark)` or the `Sparkmagic (PySpark3)` kernel and connect to a remote Amazon EMR cluster\. 
**Note**  
The EMR cluster must be configured with an IAM role that has the `AmazonSageMakerFullAccess` policy attached\. For information about configuring roles for an EMR cluster, see [Configure IAM Roles for Amazon EMR Permissions to AWS Services](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-iam-roles.html) in the *Amazon EMR Management Guide*\.

     
+ You can get the Scala library from Maven\. Add the Spark library to your project by adding the following dependency to your `pom.xml` file:

  ```
  <dependency>
      <groupId>com.amazonaws</groupId>
      <artifactId>sagemaker-spark_2.11</artifactId>
      <version>spark_2.2.0-1.0</version>
  </dependency>
  ```

## Integrate Your Apache Spark Application with SageMaker<a name="spark-sdk-common-process"></a>

The following is high\-level summary of the steps for integrating your Apache Spark application with SageMaker\.

1. Continue data preprocessing using the Apache Spark library that you are familiar with\. Your dataset remains a `DataFrame` in your Spark cluster\. Load your data into a `DataFrame` and preprocess it so that you have a `features` column with `org.apache.spark.ml.linalg.Vector` of `Doubles`, and an optional `label` column with values of `Double`​ type\.

1. Use the estimator in the SageMaker Spark library to train your model\. For example, if you choose the k\-means algorithm provided by SageMaker for model training, you call the `KMeansSageMakerEstimator.fit` method\. 

   Provide your `DataFrame` as input\. The estimator returns a `SageMakerModel` object\. 
**Note**  
`SageMakerModel` extends the `org.apache.spark.ml.Model`\.

   The `fit` method does the following: 

   1. Converts the input `DataFrame` to the protobuf format by selecting the `features` and `label` columns from the input `DataFrame` and uploading the protobuf data to an Amazon S3 bucket\. The protobuf format is efficient for model training in SageMaker\.

   1. Starts model training in SageMaker by sending a SageMaker [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\. After model training has completed, SageMaker saves the model artifacts to an S3 bucket\. 

      SageMaker assumes the IAM role that you specified for model training to perform tasks on your behalf\. For example, it uses the role to read training data from an S3 bucket and to write model artifacts to a bucket\. 

   1. Creates and returns a `SageMakerModel` object\. The constructor does the following tasks, which are related to deploying your model to SageMaker\. 

      1. Sends a [ `CreateModel`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateModel.html) request to SageMaker\. 

      1. Sends a [ `CreateEndpointConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) request to SageMaker\.

      1. Sends a [ `CreateEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpoint.html) request to SageMaker, which then launches the specified resources, and hosts the model on them\. 

1. You can get inferences from your model hosted in SageMaker with the `SageMakerModel.transform`\. 

   Provide an input `DataFrame` with features as input\. The `transform` method transforms it to a `DataFrame` containing inferences\. Internally, the `transform` method sends a request to the [ `InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_InvokeEndpoint.html) SageMaker API to get inferences\. The `transform` method appends the inferences to the input `DataFrame`\.