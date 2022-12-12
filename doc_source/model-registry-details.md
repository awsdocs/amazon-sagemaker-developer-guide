# View the Details of a Model Version<a name="model-registry-details"></a>

You can view details of a specific model version by using either the AWS SDK for Python \(Boto3\) or by using Amazon SageMaker Studio\.

## View the Details of a Model Version \(Boto3\)<a name="model-registry-details-api"></a>

To view the details of a model version by using Boto3, complete the following steps\.

1. Call the `list_model_packages` method to view the model versions in a model group\.

   ```
   sm_client.list_model_packages(ModelPackageGroupName="ModelGroup1")
   ```

   The response is a list of model package summaries\. You can get the Amazon Resource Name \(ARN\) of the model versions from this list\.

   ```
   {'ModelPackageSummaryList': [{'ModelPackageGroupName': 'AbaloneMPG-16039329888329896',
      'ModelPackageVersion': 1,
      'ModelPackageArn': 'arn:aws:sagemaker:us-east-2:123456789012:model-package/ModelGroup1/1',
      'ModelPackageDescription': 'TestMe',
      'CreationTime': datetime.datetime(2020, 10, 29, 1, 27, 46, 46000, tzinfo=tzlocal()),
      'ModelPackageStatus': 'Completed',
      'ModelApprovalStatus': 'Approved'}],
    'ResponseMetadata': {'RequestId': '12345678-abcd-1234-abcd-aabbccddeeff',
     'HTTPStatusCode': 200,
     'HTTPHeaders': {'x-amzn-requestid': '12345678-abcd-1234-abcd-aabbccddeeff',
      'content-type': 'application/x-amz-json-1.1',
      'content-length': '349',
      'date': 'Mon, 23 Nov 2020 04:56:50 GMT'},
     'RetryAttempts': 0}}
   ```

1. Call `describe_model_package` to see the details of the model version\. You pass in the ARN of a model version that you got in the output of the call to `list_model_packages`\.

   ```
   sm_client.describe_model_package(ModelPackageName="arn:aws:sagemaker:us-east-2:123456789012:model-package/ModelGroup1/1")
   ```

   The output of this call is a JSON object with the model version details\.

   ```
   {'ModelPackageGroupName': 'ModelGroup1',
    'ModelPackageVersion': 1,
    'ModelPackageArn': 'arn:aws:sagemaker:us-east-2:123456789012:model-package/ModelGroup/1',
    'ModelPackageDescription': 'Test Model',
    'CreationTime': datetime.datetime(2020, 10, 29, 1, 27, 46, 46000, tzinfo=tzlocal()),
    'InferenceSpecification': {'Containers': [{'Image': '257758044811.dkr.ecr.us-east-2.amazonaws.com/sagemaker-xgboost:1.0-1-cpu-py3',
       'ImageDigest': 'sha256:99fa602cff19aee33297a5926f8497ca7bcd2a391b7d600300204eef803bca66',
       'ModelDataUrl': 's3://sagemaker-us-east-2-123456789012/ModelGroup1/pipelines-0gdonccek7o9-AbaloneTrain-stmiylhtIR/output/model.tar.gz'}],
     'SupportedTransformInstanceTypes': ['ml.m5.xlarge'],
     'SupportedRealtimeInferenceInstanceTypes': ['ml.t2.medium', 'ml.m5.xlarge'],
     'SupportedContentTypes': ['text/csv'],
     'SupportedResponseMIMETypes': ['text/csv']},
    'ModelPackageStatus': 'Completed',
    'ModelPackageStatusDetails': {'ValidationStatuses': [],
     'ImageScanStatuses': []},
    'CertifyForMarketplace': False,
    'ModelApprovalStatus': 'PendingManualApproval',
    'LastModifiedTime': datetime.datetime(2020, 10, 29, 1, 28, 0, 438000, tzinfo=tzlocal()),
    'ResponseMetadata': {'RequestId': '12345678-abcd-1234-abcd-aabbccddeeff',
     'HTTPStatusCode': 200,
     'HTTPHeaders': {'x-amzn-requestid': '212345678-abcd-1234-abcd-aabbccddeeff',
      'content-type': 'application/x-amz-json-1.1',
      'content-length': '1038',
      'date': 'Mon, 23 Nov 2020 04:59:38 GMT'},
     'RetryAttempts': 0}}
   ```

## View the Details of a Model Version \(Amazon SageMaker Studio\)<a name="model-registry-details-studio"></a>

To view the details of a model version in Amazon SageMaker Studio, complete the following steps\.

1. Sign in to Amazon SageMaker Studio\. For more information, see [Onboard to Amazon SageMaker Domain](gs-studio-onboard.md)\.

1. In the left navigation pane, choose the **Home** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/house.png) \)\.

1. Choose **Models**, and then **Model registry**\.

1. From the model groups list, double\-click the model group you want to view\.

1. A new tab appears with a list of the model versions in the model group, as shown in the following image\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/model-versions.png)

1. In the list of model versions, double\-click the model version for which you want to view details\.

1. On the model version tab that opens, choose one of the following to see details about the model version:
   + **Activity**: Shows events for the model version, such as approval status updates\.
   + **Model quality**: Reports metrics related to your Model Monitor model quality checks, which compare model predictions to Ground Truth\. For more information about Model Monitor model quality checks, see [Monitor model quality](model-monitor-model-quality.md)\. 
   + **Explainability**: Reports metrics related to your Model Monitor feature attribution checks, which compare the relative rankings of your features in training data versus live data\. For more information about Model Monitor explainability checks, see [Monitor Feature Attribution Drift for Models in Production](clarify-model-monitor-feature-attribution-drift.md)\.
   + **Bias report**: Reports metrics related to your Model Monitor bias drift checks, which compare the distribution of live data to training data\. For more information about Model Monitor bias drift checks, see [Monitor Bias Drift for Models in Production](clarify-model-monitor-bias-drift.md)\.
   + **Inference recommender**: Provides initial instance recommendations for optimal performance based on your model and sample payloads\.
   + **Load test**: Runs load tests across your choice of instance types when you provide your specific production requirements, such as latency and throughput constraints\.
   + **Settings**: Shows information such as the project with which the model version is associated, the pipeline that generated the model, the model group, and the model's location in Amazon S3\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/model_registry/model-version-details.png)