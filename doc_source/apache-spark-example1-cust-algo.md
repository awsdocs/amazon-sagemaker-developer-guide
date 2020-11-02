# Use Custom Algorithms for Model Training and Hosting on Amazon SageMaker with Apache Spark<a name="apache-spark-example1-cust-algo"></a>

In [Example 1: Use Amazon SageMaker for Training and Inference with Apache Spark](apache-spark-example1.md), you use the `kMeansSageMakerEstimator` because the example uses the k\-means algorithm provided by Amazon SageMaker for model training\. You might choose to use your own custom algorithm for model training instead\. Assuming that you have already created a Docker image, you can create your own `SageMakerEstimator` and specify the Amazon Elastic Container Registry path for your custom image\. 

The following example shows how to create a `KMeansSageMakerEstimator` from the `SageMakerEstimator`\. In the new estimator, you explicitly specify the Docker registry path to your training and inference code images\.

```
import com.amazonaws.services.sagemaker.sparksdk.IAMRole
import com.amazonaws.services.sagemaker.sparksdk.SageMakerEstimator
import com.amazonaws.services.sagemaker.sparksdk.transformation.serializers.ProtobufRequestRowSerializer
import com.amazonaws.services.sagemaker.sparksdk.transformation.deserializers.KMeansProtobufResponseRowDeserializer

val estimator = new SageMakerEstimator(
  trainingImage =
    "811284229777.dkr.ecr.us-east-1.amazonaws.com/kmeans:1",
  modelImage =
    "811284229777.dkr.ecr.us-east-1.amazonaws.com/kmeans:1",
  requestRowSerializer = new ProtobufRequestRowSerializer(),
  responseRowDeserializer = new KMeansProtobufResponseRowDeserializer(),
  hyperParameters = Map("k" -> "10", "feature_dim" -> "784"),
  sagemakerRole = IAMRole(roleArn),
  trainingInstanceType = "ml.p2.xlarge",
  trainingInstanceCount = 1,
  endpointInstanceType = "ml.c4.xlarge",
  endpointInitialInstanceCount = 1,
  trainingSparkDataFormat = "sagemaker")
```

In the code, the parameters in the `SageMakerEstimator` constructor include:
+ `trainingImage` —Identifies the Docker registry path to the training image containing your custom code\.
+ `modelImage` —Identifies the Docker registry path to the image containing inference code\.
+ `requestRowSerializer` —Implements `com.amazonaws.services.sagemaker.sparksdk.transformation.RequestRowSerializer`\.

  This parameter serializes rows in the input `DataFrame` to send them to the model hosted in SageMaker for inference\.
+ `responseRowDeserializer` —Implements 

  `com.amazonaws.services.sagemaker.sparksdk.transformation.ResponseRowDeserializer`\.

  This parameter deserializes responses from the model, hosted in SageMaker, back into a `DataFrame`\.
+ `trainingSparkDataFormat` —Specifies the data format that Spark uses when uploading training data from a `DataFrame` to S3\. For example, `"sagemaker"` for protobuf format, `"csv"` for comma\-separated values, and `"libsvm"` for LibSVM format\. 

You can implement your own `RequestRowSerializer` and `ResponseRowDeserializer` to serialize and deserialize rows from a data format that your inference code supports, such as \.libsvm or \.\.csv\.