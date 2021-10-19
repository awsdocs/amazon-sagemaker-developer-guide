# Check Prediction Results<a name="async-inference-check-predictions"></a>

There are several ways you can check predictions results from your asynchronous endpoint\. Some options are:

1. Amazon SNS topics\.

1. Check for outputs in your Amazon S3 bucket\.

## Amazon SNS Topics<a name="async-inference-check-predictions-sns-topic"></a>

Amazon SNS is a notification service for messaging\-oriented applications, with multiple subscribers requesting and receiving "push" notifications of time\-critical messages via a choice of transport protocols, including HTTP, Amazon SQS, and email\. Amazon SageMaker Asynchronous Inference posts notifications when you create an endpoint with [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateEndpointConfig.html) and specify an Amazon SNS topic\.

**Note**  
In order to receive Amazon SNS notifications, your IAM role must have `sns:Publish` permissions\. See the LINK TO PREREQS\. for information on requirements you must satisfy to use Asynchronous Inference\.

To use Amazon SNS to check prediction results from your asynchronous endpoint, you first need to create a topic, subscribe to the topic, confirm your subscription to the topic, and note the Amazon Resource Name \(ARN\) of that topic\. For detailed information on how to create, subscribe, and find the Amazon ARN of an Amazon SNS topic, see [Configuring Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/sns-configuring.html)\.

Provide the Amazon SNS topic ARN\(s\) in the `AsyncInferenceConfig` field when you create an endpoint configuration with `CreateEndpointConfig`\. You can specify both an Amazon SNS `ErrorTopic` and an `SuccessTopic`\.

```
import boto3

sagemaker_client = boto3.client('sagemaker', region_name=<aws_region>)

sagemaker_client.create_endpoint_config(
    EndpointConfigName=<endpoint_config_name>, # You specify this name in a CreateEndpoint request.
    # List of ProductionVariant objects, one for each model that you want to host at this endpoint.
    ProductionVariants=[
        {
            "VariantName": "variant1", # The name of the production variant.
            "ModelName": "model_name", 
            "InstanceType": "ml.m5.xlarge", # Specify the compute instance type.
            "InitialInstanceCount": 1 # Number of instances to launch initially.
        }
    ],
    AsyncInferenceConfig={
        "OutputConfig": {
            # Location to upload response outputs when no location is provided in the request.
            "S3OutputPath": "s3://<bucket>/<output_directory>"
            "NotificationConfig": {
                "SuccessTopic": "arn:aws:sns:aws-region:account-id:topic-name",
                "ErrorTopic": "arn:aws:sns:aws-region:account-id:topic-name",
            }
        }
    }
)
```

## Check Your S3 Bucket<a name="async-inference-check-predictions-s3-bucket"></a>

When you invoke an endpoint with `InvokeEndpointAsync`, it returns a response object\. You can use the response object to get the Amazon S3 URI where your output is stored\. With the output location, you can use a SageMaker Python SDK SageMaker session class to programmatically check for on an output\.

The following stores the output dictionary of `InvokeEndpointAsync` as a variable named response\. With the response variable, you then get the Amazon S3 output URI and store it as a string variable called `output_location`\. 

```
import uuid
import boto3

sagemaker_runtime = boto3.client("sagemaker-runtime", region_name=<aws_region>)

# Specify the S3 URI of the input. Here, a single SVM sample
input_location = "s3://bucket-name/test_point_0.libsvm" 

response = sagemaker_runtime.invoke_endpoint_async(
    EndpointName='<endpoint-name>',
    InputLocation=input_location,
    InferenceId=str(uuid.uuid4()), 
    ContentType="text/libsvm" #Specify the content type of your data
)

output_location = response['OutputLocation']
print(f"OutputLocation: {output_location}")
```

For information about supported content types, see [Common Data Formats for Inference](cdf-inference.md)\.

With the Amazon S3 output location, you can then use a [SageMaker Python SDK SageMaker Session Class](https://sagemaker.readthedocs.io/en/stable/api/utility/session.html?highlight=session) to read in Amazon S3 files\. The following code example shows how to create a function \(`get_ouput`\) that repeatedly attempts to read a file from the Amazon S3 output location:

```
import sagemaker
import urllib, time
from botocore.exceptions import ClientError

sagemaker_session = sagemaker.session.Session()

def get_output(output_location):
    output_url = urllib.parse.urlparse(output_location)
    bucket = output_url.netloc
    key = output_url.path[1:]
    while True:
        try:
            return sagemaker_session.read_s3_file(
                                        bucket=output_url.netloc, 
                                        key_prefix=output_url.path[1:])
        except ClientError as e:
            if e.response['Error']['Code'] == 'NoSuchKey':
                print("waiting for output...")
                time.sleep(2)
                continue
            raise
            
output = get_output(output_location)
print(f"Output: {output}")
```