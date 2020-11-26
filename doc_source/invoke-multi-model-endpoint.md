# Invoke a Multi\-Model Endpoint<a name="invoke-multi-model-endpoint"></a>

To invoke a multi\-model endpoint, use the [ `invoke_endpoint`](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-runtime.html#SageMakerRuntime.Client.invoke_endpoint) from the SageMaker Runtime just as you would invoke a single model endpoint, with one change\. Pass a new `TargetModel` parameter that specifies which of the models at the endpoint to target\. The SageMaker Runtime `InvokeEndpoint` request supports `X-Amzn-SageMaker-Target-Model` as a new header that takes the relative path of the model specified for invocation\. The SageMaker system constructs the absolute path of the model by combining the prefix that is provided as part of the `CreateModel` API call with the relative path of the model\.

The following example prediction request uses the [AWS SDK for Python \(Boto 3\)](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker-runtime.html) in the sample notebook\.

```
response = runtime_sm_client.invoke_endpoint(
                        EndpointName = ’my-endpoint’,
                        ContentType  = 'text/csv',
                        TargetModel  = ’Houston_TX.tar.gz’,
                        Body         = body)
```

The multi\-model endpoint dynamically loads target models as needed\. You can observe this when running the [MME Sample Notebook](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/advanced_functionality/multi_model_xgboost_home_value/xgboost_multi_model_endpoint_home_value.ipynb) as it iterates through random invocations against multiple target models hosted behind a single endpoint\. The first request against a given model takes longer because the model has to be downloaded from Amazon Simple Storage Service \(Amazon S3\) and loaded into memory\. \(This is called a cold start\.\) Subsequent calls finish faster because there's no additional overhead after the model has loaded\.

## Retry Requests on ModelNotReadyException Errors<a name="invoke-multi-model-config-retry"></a>

The first time you call `invoke_endpoint` for a model, the model is downloaded from Amazon Simple Storage Service and loaded into the inference container\. This makes the first call take longer to return\. Subsequent calls to the same model finish faster, because the model is already loaded\.

SageMaker returns a response for a call to `invoke_endpoint` within 60 seconds\. Some models are too large to download within 60 seconds\. If the model does not finish loading before the 60 second timeout limit, the request to `invoke_endpoint` returns with the error code `ModelNotReadyException`, and the model continues to download and load into the inference container for up to 360 seconds\. If you get a `ModelNotReadyException` error code for an `invoke_endpoint` request, retry the request\. By default, the AWS SDKs for Python \(Boto 3\) \(using [Legacy retry mode](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/retries.html#legacy-retry-mode)\) and Java retry `invoke_endpoint` requests that result in `ModelNotReadyException` errors\. You can configure the retry strategy to continue retrying the request for up to 360 seconds\. If you expect your model to take longer than 60 seconds to download and load into the container, set the SDK socket timeout to 70 seconds\. For more information about configuring the retry strategy for Boto 3, see [Configuring a retry mode](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/retries.html#configuring-a-retry-mode)\. The following code shows an example that configures the retry strategy to retry calls to `invoke_endpoint` for up to 180 seconds\.

```
import boto3
from botocore.config import Config

# This example retry strategy sets the retry attempts to 2. 
# With this setting, the request can attempt to download and/or load the model 
# for upto 180 seconds: 1 orginal request (60 seconds) + 2 retries (120 seconds)
config = Config(
    read_timeout=70,
    retries={
        'max_attempts': 2  # This value can be adjusted to 5 to go up to the 360s max timeout
    }
)
runtime_sm_client = boto3.client('sagemaker-runtime', config=config)
```