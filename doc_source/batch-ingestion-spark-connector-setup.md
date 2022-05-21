# Batch Ingestion Spark Connector Setup<a name="batch-ingestion-spark-connector-setup"></a>

## Introduction<a name="w2480aac23c29c11b3"></a>

 Amazon SageMaker Feature Store supports batch data ingestion with Spark, using your existing ETL pipeline, or a pipeline on Amazon EMR\. You can also use this functionality from a Amazon SageMaker Notebook Instance\. 

 Methods for installing and implementing batch data ingestion are provided for Python and Scala\. Python developers can use the `Amazon SageMaker-feature-store-pyspark` Python library for local development, installation on Amazon EMR, or run it from Jupyter notebooks\. Scala developers can use the Feature Store Spark connector available in Maven\. 

## Installation<a name="w2480aac23c29c11b5"></a>

 **Scala Users** 

 ****Requirements**** 
+  Spark >= 3\.0\.0 
+  Scala >= 2\.12\.x  
+  Amazon EMR > 6\.x \(only if you are using Amazon EMR\) 

 **Declare the Dependency in POM\.xml** 

 The Feature Store Spark connector is available in the [Maven central repository](https://mvnrepository.com/artifact/software.amazon.sagemaker.featurestore/sagemaker-feature-store-spark-sdk)\. Declare the following in your project’s `POM.xml`: 

```
 <dependency>
 <groupId>software.amazon.sagemaker.featurestore</groupId>
 <artifactId>sagemaker-feature-store-spark-sdk_2.12</artifactId>
 <version>1.0.0</version>
 </dependency>
```

 **Python Users** 

 ****Requirements**** 
+  PySpark >= 3\.0\.0 
+  Python >= 3\.6  
+  Amazon EMR > 6\.x \(only if you are using Amazon EMR\) 

 A library is available for Python developers, [sagemaker\-feature\-store\-pyspark](https://pypi.org/project/sagemaker-feature-store-pyspark/)\. The following sections describe how to install the library locally, on Amazon EMR, and on Amazon SageMaker\. 

 We recommend setting the `$SPARK_HOME` to the directory where you have Spark installed\. During installation, we are uploading some required jar files to `SPARK_HOME`, so that all of the dependencies will load automatically\. Spark starting a JVM is required to make this PySpark library work\. 

 **Local Installation** 

 To find more info about the installation, enable verbose mode by appending `--verbose` to the following installation command\. 

```
pip3 install Amazon SageMaker-feature-store-pyspark --no-binary :all:
```

 **Installation on Amazon EMR** 

 Create the cluster with the latest version of container \(version 6\) and enable SSH for troubleshooting\.  

 You can either create a custom step to start the library installation or SSH to your cluster to install the library directly in console\. 

```
    export SPARK_HOME=/usr/lib/spark
    sudo -E pip3 install sagemaker-feature-store-pyspark --no-binary :all: --verbose
```

 Note: If you want to install the dependent jars automatically to `SPARK_HOME`, do not use Amazon EMR’s bootstrap step\. 

 **Installation on a Amazon SageMaker Notebook Instance** 

 Amazon SageMaker Notebook Instances are using older version of Spark that is not compatible with Feature Store Spark Connector\. You must upgrade Spark, then install `sagemaker-feature-store-pyspark`\.  

 Inside your notebook, add and run a cell like the following: 

```
import os
    
original_spark_version = "2.4.0"
os.environ['SPARK_HOME'] = '/home/ec2-user/anaconda3/envs/python3/lib/python3.6/site-packages/pyspark'
    
# Install a newer versiion of Spark which is compatible with spark library
!pip3 install pyspark==3.1.1
!pip3 install sagemaker-feature-store-pyspark --no-binary :all:
```

## Example Implementations<a name="w2480aac23c29c11b7"></a>

 **Scala** 

 *FeatureStoreBatchIngestion\.scala* 

```
import software.amazon.sagemaker.featurestore.sparksdk.FeatureStoreManager
import org.apache.spark.sql.types.{StringType, StructField, StructType}
import org.apache.spark.sql.{Row, SparkSession}

object ProgramOffline {
  def main(args: Array[String]): Unit = {

    val spark = SparkSession.builder().getOrCreate()

    // Construct test DataFrame
    val data = List(
      Row("1", "2021-07-01T12:20:12Z"),
      Row("2", "2021-07-02T12:20:13Z"),
      Row("3", "2021-07-03T12:20:14Z")
    )
    
    val schema = StructType(
      List(StructField("RecordIdentifier", StringType), StructField("EventTime", StringType))
    )

    val df = spark.createDataFrame(spark.sparkContext.parallelize(data), schema)
    val featureStoreManager = new FeatureStoreManager()
    
    // Load the feature definitions from input schema. The feature definitions can be used to create a feature group
    val featureDefinitions = featureStoreManager.loadFeatureDefinitionsFromSchema(df)

    val featureGroupArn = "arn:aws:Amazon SageMaker:us-west-2:<your-account-id>:feature-group/<your-feature-group-name>"
   
    // Ingest by default
    featureStoreManager.ingestData(df, featureGroupArn)
    
    // Offline store direct ingestion, flip the flag of direct_offline_store
    featureStoreManager.ingestData(df, featureGroupArn, directOfflineStore = true)
  }
}
```

 **Python** 

 *FeatureStoreBatchIngestion\.py* 

```
from pyspark.sql import SparkSession
from feature_store_pyspark.FeatureStoreManager import FeatureStoreManager
import feature_store_pyspark

extra_jars = ",".join(feature_store_pyspark.classpath_jars())
spark = SparkSession.builder \
.config("spark.jars", extra_jars) \
.getOrCreate()

# Construct test DataFrame
columns = ["RecordIdentifier", "EventTime"]
data = [("1","2021-03-02T12:20:12Z"), ("2", "2021-03-02T12:20:13Z"), ("3", "2021-03-02T12:20:14Z")]

df = spark.createDataFrame(data).toDF(*columns)
feature_store_manager= FeatureStoreManager()
 
# Load the feature definitions from input schema. The feature definitions can be used to create a feature group
feature_definitions = feature_store_manager.load_feature_definitions_from_schema(df)

feature_group_arn = "arn:aws:Amazon SageMaker:us-west-2:<your-account-id>:feature-group/<your-feature-group-name>"

# Ingest by default
feature_store_manager.ingest_data(input_data_frame=df, feature_group_arn=feature_group_arn)

# Offline store direct ingestion, flip the flag of direct_offline_store
feature_store_manager.ingest_data(input_data_frame=df, feature_group_arn=feature_group_arn, direct_offline_store=true)
```

 **Submit a Spark Job** 

 **Scala** 

 You should be able to use the Spark connector as a normal dependency\. No extra instruction is needed to run the application on all platforms\. 

 **Python** 

 The PySpark version requires an extra dependent jar to be imported, so extra steps are needed to run the Spark application\. 

 If you did not specify `SPARK_HOME` during installation, then you have to load required jars in JVM when running `spark-submit`\. `feature-store-pyspark-dependency-jars` is a Python script installed by the Spark library to automatically fetch the path to all jars for you\. 

```
spark-submit --jars `feature-store-pyspark-dependency-jars` FeatureStoreBatchIngestion.py
```

 If you are running this application on Amazon EMR, it’s recommended to run the application in client mode, so that you do not need to distribute the dependent jars to other task nodes\. Add one more step in Amazon EMR cluster with Spark argument like this: 

```
spark-submit --deploy-mode client --master yarn s3://<path-to-script>/FeatureStoreBatchIngestion.py
```