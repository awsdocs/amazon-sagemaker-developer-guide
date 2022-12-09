# Troubleshoot Inference Recommender errors<a name="inference-recommender-troubleshooting"></a>

This section contains information about how to understand and prevent common errors, the error messages they generate, and guidance on how to resolve these errors\.

## How to troubleshoot<a name="inference-recommender-troubleshooting-how-to"></a>

You can attempt to resolve your error by going through the following steps:
+ Check if you've covered all the prerequisites to use Inference Recommender\. See the [Inference Recommender Prerequisites](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender-prerequisites.html)\.
+ Check that you are able to deploy your model from Model Registry to an endpoint and that it can process your payloads without errors\. See [Deploy a Model from the Registry](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry-deploy.html)\.
+ When you kick off an Inference Recommender job, you should see endpoints being created in the console and you can review the CloudWatch logs\.

## Common errors<a name="inference-recommender-troubleshooting-common"></a>

Review the following table for common Inference Recommender errors and their solutions\.


| Error | Solution | 
| --- | --- | 
|  Specify `Domain` in the Model Package version 1\. `Domain` is a mandatory parameter for the job\.  |  Make sure you provide the ML Domain or `OTHER` if unknown\.  | 
|  Provided role ARN cannot be assumed and an `AWSSecurityTokenServiceException` error occurred\.  |  Make sure the execution role provided has the necessary permissions specified in the prerequisites\.  | 
|  Specify `Framework` in the Model Package version 1\.`Framework` is a mandatory parameter for the job\.  |  Make sure you provide the ML Framework or `OTHER` if unknown\.  | 
|  Users at the end of prev phase is 0 while initial users of current phase is 1\.  |  Users here refers to virtual users or threads used to send requests\. Each phase starts with A users and ends with B users such that B > A\. Between sequential phases, x\_1 and x\_2, we require that abs\(x\_2\.A \- x\_1\.B\) <= 3 and >= 0\.  | 
|  Total Traffic duration \(across\) should not be more than Job duration\.  |  The total duration of all your Phases cannot exceed the Job duration\.  | 
|  Burstable instance type ml\.t2\.medium is not allowed\.  |  Inference Recommender doesn't support load testing on t2 instance family because burstable instances do not provide consistent performance\.  | 
|  ResourceLimitExceeded when calling CreateEndpoint operation  |  You have exceeded a SageMaker resource limit\. For example, Inference Recommender might be unable to provision endpoints for benchmarking if the account has reached the endpoint quota\. For more information about SageMaker limits and quotas, see [Amazon SageMaker endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/sagemaker.html)\.  | 
|  ModelError when calling InvokeEndpoint operation  |  A model error can happen for the following reasons: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender-troubleshooting.html)  | 
|  PayloadError when calling InvokeEndpoint operation  |  A payload error can happen for following reasons: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender-troubleshooting.html)  | 

## Check CloudWatch<a name="inference-recommender-troubleshooting-check-cw"></a>

When you kick off an Inference Recommender job, you should see endpoints being created in the console\. Select one of the endpoints and view the CloudWatch logs to monitor for any 4xx/5xx errors\. If you have a successful Inference Recommender job, you will be able to see the endpoint names as part of the results\. Even if your Inference Recommender job is unsuccessful, you can still check the CloudWatch logs for the deleted endpoints by following the steps below:

1. Open the Amazon CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Select the Region in which you created the Inference Recommender job from the **Region** dropdown list in the top right\.

1. In the navigation pane of CloudWatch, choose **Logs**, and then select **Log groups**\.

1. Search for the log group called `/aws/sagemaker/Endpoints/sm-epc-*`\. Select the log group based on your most recent Inference Recommender job\.

## Check benchmarks<a name="inference-recommender-troubleshooting-check-benchmarks"></a>

When you kick off an Inference Recommender job, Inference Recommender creates several benchmarks to evaluate the performance of your model on different instance types\. You can use the [ListInferenceRecommendationsJobSteps](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListInferenceRecommendationsJobSteps.html) API to view the details for all the benchmarks\. If you have a failed benchmark, you can see the failure reasons as part of the results\.

To use the [ListInferenceRecommendationsJobSteps](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListInferenceRecommendationsJobSteps.html) API, provide the following values:
+ For `JobName`, provide the name of the Inference Recommender job\.
+ For `StepType`, use `BENCHMARK` to return details about the job's benchmarks\.
+ For `Status`, use `FAILED` to return details about only the failed benchmarks\. For a list of the other status types, see the `Status` field in the [ListInferenceRecommendationsJobSteps](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ListInferenceRecommendationsJobSteps.html) API\.

```
# Create a low-level SageMaker service client.
import boto3
aws_region = '<region>'
sagemaker_client = boto3.client('sagemaker', region_name=aws_region) 

# Provide the job name for the SageMaker Inference Recommender job
job_name = '<job-name>'

# Filter for benchmarks
step_type = 'BENCHMARK' 

# Filter for benchmarks that have a FAILED status
status = 'FAILED'

response = sagemaker_client.list_inference_recommendations_job_steps(
    JobName = job_name,
    StepType = step_type,
    Status = status
)
```

You can print the response object to view the results\. The preceding code example stored the response in a variable called `response`:

```
print(response)
```