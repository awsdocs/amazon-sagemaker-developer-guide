# Invoke a Multi\-Model Endpoint<a name="invoke-multi-model-endpoint"></a>

To invoke a multi\-model endpoint, use the [ `runtime_InvokeEndpoint`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_runtime_InvokeEndpoint.html) from the SageMaker Runtime just as you would invoke a single model endpoint, with one change\. Pass a new `TargetModel` parameter that specifies which of the models at the endpoint to target\. The SageMaker Runtime `InvokeEndpoint` request supports `X-Amzn-SageMaker-Target-Model` as a new header that takes the relative path of the model specified for invocation\. The SageMaker system constructs the absolute path of the model by combining the prefix that is provided as part of the `CreateModel` API call with the relative path of the model\.

The following example prediction request uses the [AWS SDK for Python \(Boto 3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-runtime.html) in the sample notebook\.

```
response = runtime_sm_client.invoke_endpoint(
                        EndpointName = ’my-endpoint’,
                        ContentType  = 'text/csv',
                        TargetModel  = ’Houston_TX.tar.gz’,
                        Body         = body)
```

The multi\-model endpoint dynamically loads target models as needed\. You can observe this when running the [MME Sample Notebook](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/multi_model_xgboost_home_value/xgboost_multi_model_endpoint_home_value.ipynb) as it iterates through random invocations against multiple target models hosted behind a single endpoint\. The first request against a given model takes longer because the model has to be downloaded from Amazon Simple Storage Service \(Amazon S3\) and loaded into memory\. \(This is called a cold start\.\) Subsequent calls finish faster because there's no additional overhead after the model has loaded\.

**Note**  
Invoking multi\-model endpoints using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) isn't supported\.