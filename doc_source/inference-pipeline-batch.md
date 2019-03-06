# Run Batch Transforms with an Inference Pipeline<a name="inference-pipeline-batch"></a>

Batch transform uses a trained model to get inferences on an entire dataset that is stored in Amazon Simple Storage Service \(Amazon S3\) For information about Amazon SageMaker batch transforms , see [Get Inferences for an Entire Dataset with Batch Transform](how-it-works-batch.md)\. The same inference pipeline model created and deployed to an endpoint for real\-time processing can also be used in a batch transform job to process inferences on a full dataset\. For a batch transform job in a pipeline, the input data is downloaded from Amazon S3 and sent in one or more HTTP requests to the inference pipeline model\. For an example that prepares data for a batch transform, see the **Preparing data for Batch Transform** section of the [ML Pipeline with SparkML and XGBoost \- Training and Inference](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/inference_pipeline_sparkml_xgboost_abalone) sample notebook\. 

**Note**  
When you use your own custom Docker images in a pipeline that includes [Amazon SageMaker built\-in algorithms](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html), you must have an [Amazon ECR policy](https://docs.aws.amazon.com/AmazonECR/latest/userguide/what-is-ecr.html)\. In particular, your repo needs to grant permission for Amazon SageMaker to pull the image\. For information on how to implement this ECR policy, see [Troubleshoot ECR Permissions for Inference Pipelines](inference-pipeline-troubleshoot.md#inference-pipeline-troubleshoot-permissions)\.

The following code shows how to run a transform job using the Amazon SageMaker Python SDK\. In this example, `model_name` refers to the inference pipeline that combines SparkML and XGBoost models, which was created in previous examples\. The Amazon S3 location specified by `input_data_path` contains input CSV data to be downloaded and sent to the SparkML model\. After the transform job completes, the Amazon S3 location specified by `output_data_path` will contain the output CSV data returned by the XGBoost model\.

```
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
    sagemaker_session=sess,
    accept = CONTENT_TYPE_CSV)
transform_job.transform(data = input_data_path, 
                        content_type = CONTENT_TYPE_CSV, 
                        split_type = 'Line')
```