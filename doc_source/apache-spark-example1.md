# Example 1: Use Amazon SageMaker for Training and Inference with Apache Spark<a name="apache-spark-example1"></a>

**Topics**
+ [Use Custom Algorithms for Model Training and Hosting on Amazon SageMaker with Apache Spark](apache-spark-example1-cust-algo.md)
+ [Use the SageMakerEstimator in a Spark Pipeline](apache-spark-example1-extend-pipeline.md)

Amazon SageMaker provides an Apache Spark library \(in both Python and Scala\) that you can use to integrate your Apache Spark applications with SageMaker\. For example, you might use Apache Spark for data preprocessing and SageMaker for model training and hosting\. For more information, see [Use Apache Spark with Amazon SageMaker](apache-spark.md)\. This section provides example code that uses the Apache Spark Scala library provided by SageMaker to train a model in SageMaker using `DataFrame`s in your Spark cluster\. The example also hosts the resulting model artifacts using SageMaker hosting services\. Specifically, this example does the following:
+ Uses the `KMeansSageMakerEstimator` to fit \(or train\) a model on data

   

  Because the example uses the k\-means algorithm provided by SageMaker to train a model, you use the `KMeansSageMakerEstimator`\. You train the model using images of handwritten single\-digit numbers \(from the MNIST dataset\)\. You provide the images as an input `DataFrame`\. For your convenience, SageMaker provides this dataset in an S3 bucket\.

   

  In response, the estimator returns a `SageMakerModel` object\.

   
+ Obtains inferences using the trained `SageMakerModel`

   

  To get inferences from a model hosted in SageMaker, you call the `SageMakerModel.transform` method\. You pass a `DataFrame` as input\. The method transforms the input `DataFrame` to another `DataFrame` containing inferences obtained from the model\. 

   

  For a given input image of a handwritten single\-digit number, the inference identifies a cluster that the image belongs to\. For more information, see [K\-Means Algorithm](k-means.md)\.

This is the example code:

```
import org.apache.spark.sql.SparkSession
import com.amazonaws.services.sagemaker.sparksdk.IAMRole
import com.amazonaws.services.sagemaker.sparksdk.algorithms
import com.amazonaws.services.sagemaker.sparksdk.algorithms.KMeansSageMakerEstimator

val spark = SparkSession.builder.getOrCreate

// load mnist data as a dataframe from libsvm
val region = "us-east-1"
val trainingData = spark.read.format("libsvm")
  .option("numFeatures", "784")
  .load(s"s3://sagemaker-sample-data-$region/spark/mnist/train/")
val testData = spark.read.format("libsvm")
  .option("numFeatures", "784")
  .load(s"s3://sagemaker-sample-data-$region/spark/mnist/test/")

val roleArn = "arn:aws:iam::account-id:role/rolename"

val estimator = new KMeansSageMakerEstimator(
  sagemakerRole = IAMRole(roleArn),
  trainingInstanceType = "ml.p2.xlarge",
  trainingInstanceCount = 1,
  endpointInstanceType = "ml.c4.xlarge",
  endpointInitialInstanceCount = 1)
  .setK(10).setFeatureDim(784)

// train
val model = estimator.fit(trainingData)

val transformedData = model.transform(testData)
transformedData.show
```

The code does the following:
+ Loads the MNIST dataset from an S3 bucket provided by SageMaker \(`awsai-sparksdk-dataset`\) into a Spark `DataFrame` \(`mnistTrainingDataFrame`\):

  ```
  // Get a Spark session.
  
  val spark = SparkSession.builder.getOrCreate
  
  // load mnist data as a dataframe from libsvm
  val region = "us-east-1"
  val trainingData = spark.read.format("libsvm")
    .option("numFeatures", "784")
    .load(s"s3://sagemaker-sample-data-$region/spark/mnist/train/")
  val testData = spark.read.format("libsvm")
    .option("numFeatures", "784")
    .load(s"s3://sagemaker-sample-data-$region/spark/mnist/test/")
  
  val roleArn = "arn:aws:iam::account-id:role/rolename"
  trainingData.show()
  ```

  The `show` method displays the first 20 rows in the data frame:

  ```
  +-----+--------------------+
  |label|            features|
  +-----+--------------------+
  |  5.0|(784,[152,153,154...|
  |  0.0|(784,[127,128,129...|
  |  4.0|(784,[160,161,162...|
  |  1.0|(784,[158,159,160...|
  |  9.0|(784,[208,209,210...|
  |  2.0|(784,[155,156,157...|
  |  1.0|(784,[124,125,126...|
  |  3.0|(784,[151,152,153...|
  |  1.0|(784,[152,153,154...|
  |  4.0|(784,[134,135,161...|
  |  3.0|(784,[123,124,125...|
  |  5.0|(784,[216,217,218...|
  |  3.0|(784,[143,144,145...|
  |  6.0|(784,[72,73,74,99...|
  |  1.0|(784,[151,152,153...|
  |  7.0|(784,[211,212,213...|
  |  2.0|(784,[151,152,153...|
  |  8.0|(784,[159,160,161...|
  |  6.0|(784,[100,101,102...|
  |  9.0|(784,[209,210,211...|
  +-----+--------------------+
  only showing top 20 rows
  ```

  In each row:
  + The `label` column identifies the image's label\. For example, if the image of the handwritten number is the digit 5, the label value is 5\. 
  + The `features` column stores a vector \(`org.apache.spark.ml.linalg.Vector`\) of `Double` values\. These are the 784 features of the handwritten number\. \(Each handwritten number is a 28 x 28\-pixel image, making 784 features\.\)

   
+ Creates a SageMaker estimator \(`KMeansSageMakerEstimator`\) 

  The `fit` method of this estimator uses the k\-means algorithm provided by SageMaker to train models using an input `DataFrame`\. In response, it returns a `SageMakerModel` object that you can use to get inferences\.
**Note**  
The `KMeansSageMakerEstimator` extends the SageMaker `SageMakerEstimator`, which extends the Apache Spark `Estimator`\. 

  ```
  val estimator = new KMeansSageMakerEstimator(
    sagemakerRole = IAMRole(roleArn),
    trainingInstanceType = "ml.p2.xlarge",
    trainingInstanceCount = 1,
    endpointInstanceType = "ml.c4.xlarge",
    endpointInitialInstanceCount = 1)
    .setK(10).setFeatureDim(784)
  ```

  The constructor parameters provide information that is used for training a model and deploying it on SageMaker:
  + `trainingInstanceType` and `trainingInstanceCount`—Identify the type and number of ML compute instances to use for model training\.

     
  + `endpointInstanceType`—Identifies the ML compute instance type to use when hosting the model in SageMaker\. By default, one ML compute instance is assumed\.

     
  + `endpointInitialInstanceCount`—Identifies the number of ML compute instances initially backing the endpoint hosting the model in SageMaker\.

     
  + `sagemakerRole`—SageMaker assumes this IAM role to perform tasks on your behalf\. For example, for model training, it reads data from S3 and writes training results \(model artifacts\) to S3\. 
**Note**  
This example implicitly creates a SageMaker client\. To create this client, you must provide your credentials\. The API uses these credentials to authenticate requests to SageMaker\. For example, it uses the credentials to authenticate requests to create a training job and API calls for deploying the model using SageMaker hosting services\.
  + After the `KMeansSageMakerEstimator` object has been created, you set the following parameters, are used in model training: 
    + The number of clusters that the k\-means algorithm should create during model training\. You specify 10 clusters, one for each digit, 0 through 9\. 
    + Identifies that each input image has 784 features \(each handwritten number is a 28 x 28\-pixel image, making 784 features\)\.

     
+ Calls the estimator `fit` method

  ```
  // train
  val model = estimator.fit(trainingData)
  ```

  You pass the input `DataFrame` as a parameter\. The model does all the work of training the model and deploying it to SageMaker\. For more information see, [Integrate Your Apache Spark Application with SageMaker](apache-spark.md#spark-sdk-common-process)\. In response, you get a `SageMakerModel` object, which you can use to get inferences from your model deployed in SageMaker\. 

   

  You provide only the input `DataFrame`\. You don't need to specify the registry path to the k\-means algorithm used for model training because the `KMeansSageMakerEstimator` knows it\.

   
+ Calls the `SageMakerModel.transform` method to get inferences from the model deployed in SageMaker\.

  The `transform` method takes a `DataFrame` as input, transforms it, and returns another `DataFrame` containing inferences obtained from the model\. 

  ```
  val transformedData = model.transform(testData)
  transformedData.show
  ```

  For simplicity, we use the same `DataFrame` as input to the `transform` method that we used for model training in this example\. The `transform` method does the following:
  + Serializes the `features` column in the input `DataFrame` to protobuf and sends it to the SageMaker endpoint for inference\.
  + Deserializes the protobuf response into the two additional columns \(`distance_to_cluster` and `closest_cluster`\) in the transformed `DataFrame`\.

  The `show` method gets inferences to the first 20 rows in the input `DataFrame`: 

  ```
  +-----+--------------------+-------------------+---------------+
  |label|            features|distance_to_cluster|closest_cluster|
  +-----+--------------------+-------------------+---------------+
  |  5.0|(784,[152,153,154...|  1767.897705078125|            4.0|
  |  0.0|(784,[127,128,129...|  1392.157470703125|            5.0|
  |  4.0|(784,[160,161,162...| 1671.5711669921875|            9.0|
  |  1.0|(784,[158,159,160...| 1182.6082763671875|            6.0|
  |  9.0|(784,[208,209,210...| 1390.4002685546875|            0.0|
  |  2.0|(784,[155,156,157...|  1713.988037109375|            1.0|
  |  1.0|(784,[124,125,126...| 1246.3016357421875|            2.0|
  |  3.0|(784,[151,152,153...|  1753.229248046875|            4.0|
  |  1.0|(784,[152,153,154...|  978.8394165039062|            2.0|
  |  4.0|(784,[134,135,161...|  1623.176513671875|            3.0|
  |  3.0|(784,[123,124,125...|  1533.863525390625|            4.0|
  |  5.0|(784,[216,217,218...|  1469.357177734375|            6.0|
  |  3.0|(784,[143,144,145...|  1736.765869140625|            4.0|
  |  6.0|(784,[72,73,74,99...|   1473.69384765625|            8.0|
  |  1.0|(784,[151,152,153...|    944.88720703125|            2.0|
  |  7.0|(784,[211,212,213...| 1285.9071044921875|            3.0|
  |  2.0|(784,[151,152,153...| 1635.0125732421875|            1.0|
  |  8.0|(784,[159,160,161...| 1436.3162841796875|            6.0|
  |  6.0|(784,[100,101,102...| 1499.7366943359375|            7.0|
  |  9.0|(784,[209,210,211...| 1364.6319580078125|            6.0|
  +-----+--------------------+-------------------+---------------+
  ```

  You can interpret the data, as follows:
  + A handwritten number with the `label` 5 belongs to cluster 4 \(`closest_cluster`\)\.
  + A handwritten number with the `label` 0 belongs to cluster 5\.
  + A handwritten number with the `label` 4 belongs to cluster 9\.
  + A handwritten number with the `label` 1 belongs to cluster 6\.

For more information on how to run these examples, see [https://github\.com/aws/sagemaker\-spark/blob/master/README\.md](https://github.com/aws/sagemaker-spark/blob/master/README.md) on GitHub\.