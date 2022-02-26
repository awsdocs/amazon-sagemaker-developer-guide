# Configure a SageMaker Clarify Processing Job Container's Input and Output Parameters<a name="clarify-processing-job-configure-parameters"></a>

The Processing Job requires that you specify the following input parameters: a dataset files with input name `"dataset"` as Amazon S3 object or prefix, and an analysis configuration file with input name `"analysis_config"` as an Amazon S3 object\. The job also requires an output parameter: the output location as an Amazon S3 prefix\.

You can create and run a processing job with SageMaker [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateProcessingJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateProcessingJob.html) API using the AWS SDK or CLI or [SageMaker Python SDK](https://sagemaker.readthedocs.io/)\.

Using SageMaker Python SDK, create a `Processor` using the SageMaker Clarify container image URI:

```
from sagemaker import clarify
clarify_processor = clarify.SageMakerClarifyProcessor(role=role,
    instance_count=1,
    instance_type='ml.c5.xlarge',
    max_runtime_in_seconds=1200,
    volume_size_in_gb=100)
```

After you finish creating the Clarify processor, you need to set up the input and output object for the processor\. 

**Note**  
If you provide the `"dataset_uri"` through the "analysis\_config\.json" \(see the following topic at [Configure the Analysis](clarify-processing-job-configure-analysis.md)\), you don't need to create the `dataset_input` object\.

```
    dataset_path = "s3://my_bucket/my_folder/train.csv"
    analysis_config_path = "s3://my_bucket/my_folder/analysis_config.json"
    analysis_result_path = "s3://my_bucket/my_folder/output"
    
    analysis_config_input = ProcessingInput(
        input_name="analysis_config",
        source=analysis_config_path,
        destination="/opt/ml/processing/input/config",
        s3_data_type="S3Prefix",
        s3_input_mode="File",
        s3_compression_type="None")
    dataset_input = ProcessingInput(
        input_name="dataset",
        source=dataset_path,
        destination="/opt/ml/processing/input/data",
        s3_data_type="S3Prefix",
        s3_input_mode="File",
        s3_compression_type="None")
    analysis_result_output = ProcessingOutput(
        source="/opt/ml/processing/output",
        destination=analysis_result_path,
        output_name="analysis_result",
        s3_upload_mode="EndOfJob")
```