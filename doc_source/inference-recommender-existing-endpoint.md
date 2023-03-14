# Get an instance recommendation for an existing endpoint<a name="inference-recommender-existing-endpoint"></a>

Inference recommendation jobs run a set of load tests on recommended instance types and existing endpoint\. Inference recommendation jobs use performance metrics that are based on load tests using the sample data you provided during model version registration\.

You can benchmark and get instance recommendations for an existing SageMaker Inference endpoint to help you improve the performance of your endpoint\. The procedure of getting recommendations for an existing SageMaker Inference endpoint is similar to the procedure for [getting instance recommendations](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender-instance-recommendation.html) without an endpoint\. There are several feature exclusions to take note of when benchmarking an existing endpoint:
+ You can only use one existing endpoint per Inference Recommender job\.
+ You can only have one variant on your endpoint\.
+ You can’t use an endpoint that enables autoscaling\.
+ This functionality is only supported for [Real\-Time Inference](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints.html)\.
+ This functionality doesn’t support [Real\-Time Multi\-Model Endpoints](https://docs.aws.amazon.com/sagemaker/latest/dg/multi-model-endpoints.html)\.

**Warning**  
We strongly recommend that you don't run an Inference Recommender job on a production endpoint that handles live traffic\. The synthetic load during benchmarking can affect your production endpoint and cause throttling or provide inaccurate benchmark results\. We recommend that you use a non\-production or developer endpoint for comparison purposes\. 

The following sections demonstrate how to use Amazon SageMaker Inference Recommender to create an instance recommendation for an existing endpoint based on your model type using the AWS SDK for Python \(Boto3\) and the AWS CLI\.

**Note**  
Before you create an Inference Recommender recommendation job, make sure you have satisfied the [Prerequisites](inference-recommender-prerequisites.md)\.

## Prerequisites<a name="inference-recommender-existing-endpoint-prerequisites"></a>

If you don’t already have a SageMaker Inference endpoint, you can either [get an instance recommendation](https://docs.aws.amazon.com/sagemaker/latest/dg/inference-recommender-instance-recommendation.html) without an endpoint, or you can create a Real\-Time Inference endpoint by following the instructions in [Create your endpoint and deploy your model](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-deployment.html)\.

## Create an instance recommendation job for an existing endpoint<a name="inference-recommender-existing-endpoint-create"></a>

Create an instance recommendation programmatically using AWS SDK for Python \(Boto3\), or the AWS CLI\. Specify a job name for your instance recommendation, the name of an existing SageMaker Inference endpoint, an AWS IAM role ARN, an input configuration, and your model package ARN from when you registered your model with the model registry\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateInferenceRecommendationsJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateInferenceRecommendationsJob.html) API to get an instance endpoint recommendation\. Set the `JobType` field to `'Default'` for instance endpoint recommendation jobs\. In addition, provide the following:
+ Provide a name for your Inference Recommender recommendation job for the `JobName` field\. The Inference Recommender job name must be unique within the AWS Region and within your AWS account\.
+ The Amazon Resource Name \(ARN\) of an IAM role that enables Inference Recommender to perform tasks on your behalf\. Define this for the `RoleArn` field\.
+ The ARN of the versioned model package you created when you registered your model with the model registry\. Define this for `ModelPackageVersionArn` in the `InputConfig` field\.
+ Provide the name of an existing SageMaker Inference endpoint that you want to benchmark in Inference Recommender for `Endpoints` in the `InputConfig` field\.

Import the AWS SDK for Python \(Boto3\) package and create a SageMaker client object using the client class\. If you followed the steps in the **Prerequisites** section, the model package group ARN was stored in a variable named `model_package_arn`\.

```
# Create a low-level SageMaker service client.
import boto3
aws_region = '<region>'
sagemaker_client = boto3.client('sagemaker', region_name=aws_region) 

# Provide your model package ARN that was created when you registered your 
# model with Model Registry 
model_package_arn = '<model-package-arn>'

# Provide a unique job name for SageMaker Inference Recommender job
job_name = '<job-name>'

# Inference Recommender job type. Set to Default to get an initial recommendation
job_type = 'Default'

# Provide an IAM Role that gives SageMaker Inference Recommender permission to 
# access AWS services
role_arn = '<arn:aws:iam::<account>:role/*>'
                                    
# Provide endpoint name for your endpoint that want to benchmark in Inference Recommender
endpoint_name = '<existing-endpoint-name>'

sagemaker_client.create_inference_recommendations_job(
    JobName = job_name,
    JobType = job_type,
    RoleArn = role_arn,
    InputConfig = {
        'ModelPackageVersionArn': model_package_arn,
        'Endpoints': ['EndpointName': endpoint_name]
    }
)
```

See the [Amazon SageMaker API Reference Guide](https://docs.aws.amazon.com/sagemaker/latest/APIReference/Welcome.html) for a full list of optional and required arguments you can pass to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateInferenceRecommendationsJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateInferenceRecommendationsJob.html)\.

------
#### [ AWS CLI ]

Use the `create-inference-recommendations-job` API to get an instance endpoint recommendation\. Set the `job-type` field to `'Default'` for instance endpoint recommendation jobs\. In addition, provide the following:
+ Provide a name for your Inference Recommender recommendation job for the `job-name` field\. The Inference Recommender job name must be unique within the AWS Region and within your AWS account\.
+ The Amazon Resource Name \(ARN\) of an IAM role that enables Amazon SageMaker Inference Recommender to perform tasks on your behalf\. Define this for the `role-arn` field\.
+ The ARN of the versioned model package you created when you registered your model with Model Registry\. Define this for `ModelPackageVersionArn` in the `input-config` field\.
+ Provide the name of an existing SageMaker Inference endpoint that you want to benchmark in Inference Recommender for `Endpoints` in the `input-config` field\.

```
aws sagemaker create-inference-recommendations-job 
    --region <region>\
    --job-name <job_name>\
    --job-type Default\
    --role-arn arn:aws:iam::<account:role/*>\
    --input-config "{
        \"ModelPackageVersionArn\": \"arn:aws:sagemaker:<region:account:role/*>\",
        \"Endpoints\": [{\"EndpointName\": <endpoint_name>}]
        }"
```

------

## Get your instance recommendation job results<a name="inference-recommender-existing-endpoint-results"></a>

You can collect the results of your instance recommendation job programmatically with the same procedure for standard instance recommendation jobs\. For more information, see [Get Your Instance Recommendation Job Results](inference-recommender-instance-recommendation.md#instance-recommendation-results)\.

When you get instance recommendation job results for an existing endpoint, you should receive a JSON response similar to the following:

```
{
    "JobName": "job-name",
    "JobType": "Default",
    "JobArn": "arn:aws:sagemaker:region:account-id:inference-recommendations-job/resource-id",
    "RoleArn": "iam-role-arn",
    "Status": "COMPLETED",
    "CreationTime": 1664922919.2,
    "LastModifiedTime": 1664924208.291,
    "InputConfig": {
        "ModelPackageVersionArn": "arn:aws:sagemaker:region:account-id:model-package/resource-id",
        "Endpoints": [
            {
                "EndpointName": "endpoint-name"
            }
        ]
    },
    "InferenceRecommendations": [
        {
            "Metrics": {
                "CostPerHour": 0.7360000014305115,
                "CostPerInference": 7.456940238625975e-06,
                "MaxInvocations": 1645,
                "ModelLatency": 171
            },
            "EndpointConfiguration": {
                "EndpointName": "sm-endpoint-name",
                "VariantName": "variant-name",
                "InstanceType": "ml.g4dn.xlarge",
                "InitialInstanceCount": 1
            },
            "ModelConfiguration": {
                "EnvironmentParameters": [
                    {
                        "Key": "TS_DEFAULT_WORKERS_PER_MODEL",
                        "ValueType": "string",
                        "Value": "4"
                    }
                ]
            }
        }
    ],
    "EndpointPerformances": [
        {
            "Metrics": {
                "MaxInvocations": 184,
                "ModelLatency": 1312
            },
            "EndpointConfiguration": {
                "EndpointName": "endpoint-name"
            }
        }
    ]
}
```

The first few lines provide information about the instance recommendation job itself\. This includes the job name, role ARN, and creation and latest modification times\.

The `InferenceRecommendations` dictionary contains a list of Inference Recommender instance recommendations\.

The `EndpointConfiguration` nested dictionary contains the instance type \(`InstanceType`\) recommendation along with the endpoint and variant name \(a deployed AWS machine learning model\) that was used during the recommendation job\.

The `Metrics` nested dictionary contains information about the estimated cost per hour \(`CostPerHour`\) for your real\-time endpoint in US dollars, the estimated cost per inference \(`CostPerInference`\) in US dollars for your real\-time endpoint, the expected maximum number of `InvokeEndpoint` requests per minute sent to the endpoint \(`MaxInvocations`\), and the model latency \(`ModelLatency`\), which is the interval of time \(in milliseconds\) that your model took to respond to SageMaker\. The model latency includes the local communication times taken to send the request and to fetch the response from the container of a model and the time taken to complete the inference in the container\.

The `EndpointPerformances` nested dictionary contains the name of your existing endpoint on which the recommendation job was run \(`EndpointName`\) and the performance metrics for your endpoint \(`MaxInvocations` and `ModelLatency`\)\.

## Stop your instance endpoint recommendation<a name="inference-recommender-existing-endpoint-stop"></a>

You can stop your Inference Recommender instance recommendation jobs programmatically with the same procedure for standard instance recommendation jobs\. For more information, see [Stop Your Instance Endpoint Recommendation](inference-recommender-instance-recommendation.md#instance-recommendation-stop)\.