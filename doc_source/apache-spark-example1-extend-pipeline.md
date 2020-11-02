# Use the SageMakerEstimator in a Spark Pipeline<a name="apache-spark-example1-extend-pipeline"></a>

You can use `org.apache.spark.ml.Estimator` estimators and `org.apache.spark.ml.Model` models, and `SageMakerEstimator` estimators and `SageMakerModel` models in `org.apache.spark.ml.Pipeline` pipelines, as shown in the following example:

```
import org.apache.spark.ml.Pipeline
import org.apache.spark.ml.feature.PCA
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

// substitute your SageMaker IAM role here
val roleArn = "arn:aws:iam::account-id:role/rolename"

val pcaEstimator = new PCA()
  .setInputCol("features")
  .setOutputCol("projectedFeatures")
  .setK(50)

val kMeansSageMakerEstimator = new KMeansSageMakerEstimator(
  sagemakerRole = IAMRole(integTestingRole),
  requestRowSerializer =
    new ProtobufRequestRowSerializer(featuresColumnName = "projectedFeatures"),
  trainingSparkDataFormatOptions = Map("featuresColumnName" -> "projectedFeatures"),
  trainingInstanceType = "ml.p2.xlarge",
  trainingInstanceCount = 1,
  endpointInstanceType = "ml.c4.xlarge",
  endpointInitialInstanceCount = 1)
  .setK(10).setFeatureDim(50)

val pipeline = new Pipeline().setStages(Array(pcaEstimator, kMeansSageMakerEstimator))

// train
val pipelineModel = pipeline.fit(trainingData)

val transformedData = pipelineModel.transform(testData)
transformedData.show()
```

The parameter `trainingSparkDataFormatOptions` configures Spark to serialize to protobuf the "projectedFeatures" column for model training\. Additionally, Spark serializes to protobuf the "label" column by default\.

Because we want to make inferences using the "projectedFeatures" column, we pass the column name into the `ProtobufRequestRowSerializer`\.

The following example shows a transformed `DataFrame`:

```
+-----+--------------------+--------------------+-------------------+---------------+
|label|            features|   projectedFeatures|distance_to_cluster|closest_cluster|
+-----+--------------------+--------------------+-------------------+---------------+
|  5.0|(784,[152,153,154...|[880.731433034386...|     1500.470703125|            0.0|
|  0.0|(784,[127,128,129...|[1768.51722024166...|      1142.18359375|            4.0|
|  4.0|(784,[160,161,162...|[704.949236329314...|  1386.246826171875|            9.0|
|  1.0|(784,[158,159,160...|[-42.328192193771...| 1277.0736083984375|            5.0|
|  9.0|(784,[208,209,210...|[374.043902028333...|   1211.00927734375|            3.0|
|  2.0|(784,[155,156,157...|[941.267714528850...|  1496.157958984375|            8.0|
|  1.0|(784,[124,125,126...|[30.2848596410594...| 1327.6766357421875|            5.0|
|  3.0|(784,[151,152,153...|[1270.14374062052...| 1570.7674560546875|            0.0|
|  1.0|(784,[152,153,154...|[-112.10792566485...|     1037.568359375|            5.0|
|  4.0|(784,[134,135,161...|[452.068280676606...| 1165.1236572265625|            3.0|
|  3.0|(784,[123,124,125...|[610.596447285397...|  1325.953369140625|            7.0|
|  5.0|(784,[216,217,218...|[142.959601818422...| 1353.4930419921875|            5.0|
|  3.0|(784,[143,144,145...|[1036.71862533658...| 1460.4315185546875|            7.0|
|  6.0|(784,[72,73,74,99...|[996.740157435754...| 1159.8631591796875|            2.0|
|  1.0|(784,[151,152,153...|[-107.26076167417...|   960.963623046875|            5.0|
|  7.0|(784,[211,212,213...|[619.771820430940...|   1245.13623046875|            6.0|
|  2.0|(784,[151,152,153...|[850.152101817161...|  1304.437744140625|            8.0|
|  8.0|(784,[159,160,161...|[370.041887230547...| 1192.4781494140625|            0.0|
|  6.0|(784,[100,101,102...|[546.674328209335...|    1277.0908203125|            2.0|
|  9.0|(784,[209,210,211...|[-29.259112927426...| 1245.8182373046875|            6.0|
+-----+--------------------+--------------------+-------------------+---------------+
```