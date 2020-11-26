# Run Batch Transforms with Inference Pipelines<a name="inference-pipeline-batch"></a>

To get inferences on an entire dataset you run a batch transform on a trained model\. To run inferences on a full dataset, you can use the same inference pipeline model created and deployed to an endpoint for real\-time processing in a batch transform job\. To run a batch transform job in a pipeline, you download the input data from Amazon S3 and send it in one or more HTTP requests to the inference pipeline model\. For an example that shows how to prepare data for a batch transform, see the "Preparing Data for Batch Transform" section of the [ML Pipeline with SparkML and XGBoost \- Training and Inference sample notebook](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/inference_pipeline_sparkml_xgboost_abalone)\. For information about Amazon SageMaker batch transforms, see [Get Inferences for an Entire Dataset with Batch Transform](how-it-works-batch.md)\. 

**Note**  
To use custom Docker images in a pipeline that includes [Amazon SageMaker built\-in algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html), you need an [Amazon Elastic Container Registry \(ECR\) policy](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)\. Your Amazon ECR repository must grant SageMaker permission to pull the image\. For more information, see [Troubleshoot Amazon ECR Permissions for Inference Pipelines](inference-pipeline-troubleshoot.md#inference-pipeline-troubleshoot-permissions)\.

The following example shows how to run a transform job using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. In this example, `model_name` is the inference pipeline that combines SparkML and XGBoost models \(created in previous examples\)\. The Amazon S3 location specified by `input_data_path` contains the input data, in CSV format, to be downloaded and sent to the Spark ML model\. After the transform job has finished, the Amazon S3 location specified by `output_data_path` contains the output data returned by the XGBoost model in CSV format\.

```
import sagemaker
input_data_path = 's3://{}/{}/{}'.format(default_bucket, 'key', 'file_name')
output_data_path = 's3://{}/{}'.format(default_bucket, 'key')
transform_job = sagemaker.transformer.Transformer(
    model_name = model_name,
    instance_count = 1,
    instance_type = 'ml.m4.xlarge',
    strategy = 'SingleRecord',
    assemble_with = 'Line',
    output_path = output_data_path,
    base_transform_job_name='inference-pipelines-batch',
    sagemaker_session=sagemaker.Session(),
    accept = CONTENT_TYPE_CSV)
transform_job.transform(data = input_data_path, 
                        content_type = CONTENT_TYPE_CSV, 
                        split_type = 'Line')
```