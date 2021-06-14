# Data Processing with Apache Spark<a name="use-spark-processing-container"></a>

 Apache Spark is a unified analytics engine for large\-scale data processing\. Amazon SageMaker provides prebuilt Docker images that include Apache Spark and other dependencies needed to run distributed data processing jobs\. With the [Amazon SageMaker Python SDK](https://github.com/aws/sagemaker-python-sdk#installing-the-sagemaker-python-sdk), you can easily apply data transformations and extract features \(feature engineering\) using the Spark framework\. For information about using the SageMaker Python SDK to run Spark processing jobs, see [Data Processing with Spark](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_processing.html#data-processing-with-spark) in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/)\. 

 

A code repository that contains the source code and Dockerfiles for the Spark images is available on [GitHub](https://github.com/aws/sagemaker-spark-container)\. 

## Running a Spark Processing Job<a name="use-spark-processing-container-how-to"></a>

 You can use the [ `sagemaker.spark.PySparkProcessor`](https://sagemaker.readthedocs.io/en/stable/api/training/processing.html#sagemaker.spark.processing.PySparkProcessor) or [ `sagemaker.spark.SparkJarProcessor`](https://sagemaker.readthedocs.io/en/stable/api/training/processing.html#sagemaker.spark.processing.SparkJarProcessor) class to run your Spark application inside of a processing job\. Note you can set MaxRuntimeInSeconds to a maximum runtime limit of 5 days\. With respect to execution time, and number of instances used, simple spark workloads see a near linear relationship between the number of instances vs\. time to completion\. 

 The following code example shows how to run a processing job that invokes your PySpark script `preprocess.py`\. 

```
from sagemaker.spark.processing import PySparkProcessor

spark_processor = PySparkProcessor(
    base_job_name="spark-preprocessor",
    framework_version="2.4",
    role=role,
    instance_count=2,
    instance_type="ml.m5.xlarge",
    max_runtime_in_seconds=1200,
)

spark_processor.run(
    submit_app="preprocess.py",
    arguments=['s3_input_bucket', bucket,
               's3_input_key_prefix', input_prefix,
               's3_output_bucket', bucket,
               's3_output_key_prefix', output_prefix]
)
```

 For an in\-depth look, see the Distributed Data Processing with Apache Spark and SageMaker Processing [example notebook](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_processing/spark_distributed_data_processing/sagemaker-spark-processing.html)\. 

 If you are not using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io/) and one of its Processor classes to retrieve the pre\-built images, you can retrieve these images yourself\. The SageMaker prebuilt Docker images are stored in Amazon Elastic Container Registry \(Amazon ECR\)\. For a complete list of the available pre\-built Docker images, see the [available images](https://github.com/aws/sagemaker-spark-container/blob/master/available_images.md) document\. 

 To learn more about using the SageMaker Python SDK with Processing containers, see [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/)\. 