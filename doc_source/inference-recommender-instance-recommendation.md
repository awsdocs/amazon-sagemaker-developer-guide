# Get an instance recommendation<a name="inference-recommender-instance-recommendation"></a>

Instance recommendation jobs run a set of load tests on recommended instance types\. Inference recommendation jobs use performance metrics that are based on load tests using the sample data you provided during model version registration\.

**Note**  
Before you create an Inference Recommender recommendation job, make sure you have satisfied the [Prerequisites](inference-recommender-prerequisites.md)\.

The following demonstrates how to use Amazon SageMaker Inference Recommender to create an instance recommendation based on your model type using the AWS SDK for Python \(Boto3\), AWS CLI, and Amazon SageMaker Studio\.

## Create an instance recommendation<a name="instance-recommendation-create"></a>

Create an instance recommendation programmaticially using AWS SDK for Python \(Boto3\), with the AWS CLI, or interactively using Studio\. Specify a job name for your instance recommendation, an AWS IAM role ARN, an input configuration, and your model package ARN when you registered your model with the model registry\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Use the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateInferenceRecommendationsJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateInferenceRecommendationsJob.html) API to get an instance endpoint recommendation\. Set the `JobType` field to `'Default'` for instance endpoint recommendation jobs\. In addition, provide the following:
+ The Amazon Resource Name \(ARN\) of an IAM role that enables Inference Recommender to perform tasks on your behalf\. Define this for the `RoleArn` field\.
+ The ARN of the versioned model package you created when you registered your model with the model registry\. Define this for `ModelPackageVersionArn` in the `InputConfig` field\.
+ Provide a name for your Inference Recommender recommendation job for the `JobName` field\. The Inference Recommender job name must be unique within the AWS Region and within your AWS account\.

Import the AWS SDK for Python \(Boto3\) package and create a SageMaker client object using the client class\. If you followed the steps in the **Prerequisites** section, the model package group ARN was stored in a variable named `model_package_arn`\.

```
# Create a low-level SageMaker service client.
import boto3
aws_region = '<INSERT>'
sagemaker_client = boto3.client('sagemaker', region_name=aws_region) 

# Provide your model package ARN that was created when you registered your 
# model with Model Registry 
model_package_arn = '<INSERT>'

# Provide a unique job name for SageMaker Inference Recommender job
job_name = '<INSERT>'

# Inference Recommender job type. Set to Default to get an initial recommendation
job_type = 'Default'

# Provide an IAM Role that gives SageMaker Inference Recommender permission to 
# access AWS services
role_arn = 'arn:aws:iam::<account>:role/*'

sagemaker_client.create_inference_recommendations_job(
    JobName = job_name,
    JobType = job_type,
    RoleArn = role_arn,
    InputConfig = {
        'ModelPackageVersionArn': model_package_arn
    }
)
```

See the [Amazon SageMaker API Reference Guide](https://docs.aws.amazon.com/sagemaker/latest/APIReference/Welcome.html) for a full list of optional and required arguments you can pass to [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateInferenceRecommendationsJob.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateInferenceRecommendationsJob.html)\.

------
#### [ AWS CLI ]

Use the `create-inference-recommendations-job` API to get an instance endpoint recommendation\. Set the `job-type` field to `'Default'` for instance endpoint recommendation jobs\. In addition, provide the following:
+ The Amazon Resource Name \(ARN\) of an IAM role that enables Amazon SageMaker Inference Recommender to perform tasks on your behalf\. Define this for the `role-arn` field\.
+ The ARN of the versioned model package you created when you registered your model with Model Registry\. Define this for `ModelPackageVersionArn` in the `input-config` field\.
+ Provide a name for your Inference Recommender recommendation job for the `job-name` field\. The Inference Recommender job name must be unique within the AWS Region and within your AWS account\.

```
aws sagemaker create-inference-recommendations-job 
    --region <region>\
    --job-name <job_name>\
    --job-type Default\
    --role-arn arn:aws:iam::<account:role/*>\
    --input-config "{
        \"ModelPackageVersionArn\": \"arn:aws:sagemaker:<region:account:role/*>\",
        }"
```

------
#### [ Amazon SageMaker Studio ]

Create an instance recommendation job in Studio\.

1. In the left sidebar of Amazon SageMaker, choose the **Components and registries** icon\.

1. Select **Model Registry** from the dropdown list to display models you have registered with the model registry\.

   The left panel displays a list of model groups\. The list includes all the model groups registered with the model registry in your account, including models registered outside of Studio\.

1. Select the name of your model group\. When you select your model group, the right pane of Studio displays column heads such as **Versions** and **Setting**\.

   If you have one or more model packages within your model group, you will see a list of those model packages within the **Versions** column\.

1. Choose the **Inference recommender** column\.

1. Choose an IAM role that grants Inference Recommender permission to access AWS services\. You can create a role and attach the `AmazonSageMakerFullAccess` IAM managed policy to accomplish this\. Or you can let Studio create a role for you\.

1. Choose **Get recommendations**\.

   The instance recommendation can take up to 45 minutes\.
**Warning**  
Do not close this tab\. If you close this tab, you cancel the instance recommendation job\.

------

## Get Your Instance Recommendation Job Results<a name="instance-recommendation-results"></a>

Collect the results of your instance recommendation job programmatically with AWS SDK for Python \(Boto3\), the AWS CLI, or Studio\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Once an instance recommendation is complete, you can use `DescribeInferenceRecommendationsJob` to get the job details and recommended instance types\. Provide the job name that you used when you created the instance recommendation job\.

```
job_name='<INSERT>'
response = sagemaker_client.describe_inference_recommendations_job(
                    JobName=job_name)
```

Print the response object\. The previous code sample stored the response in a variable name `response`\.

```
print(response['Status'])
```

This returns a JSON response similar to the following:

```
{
    'JobName': 'job-name', 
    'JobDescription': 'job-description', 
    'JobType': 'Default', 
    'JobArn': 'arn:aws:sagemaker:region:account-id:inference-recommendations-job/resource-id', 
    'Status': 'COMPLETED', 
    'CreationTime': datetime.datetime(2021, 10, 26, 20, 4, 57, 627000, tzinfo=tzlocal()), 
    'LastModifiedTime': datetime.datetime(2021, 10, 26, 20, 25, 1, 997000, tzinfo=tzlocal()), 
    'InputConfig': {
                'ModelPackageVersionArn': 'arn:aws:sagemaker:region:account-id:model-package/resource-id', 
                'JobDurationInSeconds': 0
                }, 
    'InferenceRecommendations': [{
            'Metrics': {
                'CostPerHour': 0.20399999618530273, 
                'CostPerInference': 5.246913588052848e-06, 
                'MaximumInvocations': 648, 
                'ModelLatency': 263596
                }, 
            'EndpointConfiguration': {
                'EndpointName': 'endpoint-name', 
                'VariantName': 'variant-name', 
                'InstanceType': 'ml.c5.xlarge', 
                'InitialInstanceCount': 1
                }, 
            'ModelConfiguration': {
                'Compiled': False, 
                'EnvironmentParameters': []
                }
         }, 
         {
            'Metrics': {
                'CostPerHour': 0.11500000208616257, 
                'CostPerInference': 2.92620870823157e-06, 
                'MaximumInvocations': 655, 
                'ModelLatency': 826019
                }, 
            'EndpointConfiguration': {
                'EndpointName': 'endpoint-name', 
                'VariantName': 'variant-name', 
                'InstanceType': 'ml.c5d.large', 
                'InitialInstanceCount': 1
                }, 
            'ModelConfiguration': {
                'Compiled': False, 
                'EnvironmentParameters': []
                }
            }, 
            {
                'Metrics': {
                    'CostPerHour': 0.11500000208616257, 
                    'CostPerInference': 3.3625731248321244e-06, 
                    'MaximumInvocations': 570, 
                    'ModelLatency': 1085446
                    }, 
                'EndpointConfiguration': {
                    'EndpointName': 'endpoint-name', 
                    'VariantName': 'variant-name', 
                    'InstanceType': 'ml.m5.large', 
                    'InitialInstanceCount': 1
                    }, 
                'ModelConfiguration': {
                    'Compiled': False, 
                    'EnvironmentParameters': []
                    }
            }], 
    'ResponseMetadata': {
        'RequestId': 'request-id', 
        'HTTPStatusCode': 200, 
        'HTTPHeaders': {
            'x-amzn-requestid': 'x-amzn-requestid', 
            'content-type': 'content-type', 
            'content-length': '1685', 
            'date': 'Tue, 26 Oct 2021 20:31:10 GMT'
            }, 
        'RetryAttempts': 0
        }
}
```

The first few lines provide information about the instance recommendation job itself\. This includes the job name, role ARN, and creation and deletion times\. 

The `InferenceRecommendations` dictionary contains a list of Inference Recommender instance recommendations\.

The `EndpointConfiguration` nested dictionary contains the instance type \(`InstanceType`\) recommendation along with the endpoint and variant name \(a deployed AWS machine learning model\) that was used during the recommendation job\. You can use the endpoint and variant name for monitoring in Amazon CloudWatch Events\. See [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md) for more information\.

The `Metrics` nested dictionary contains information about the estimated cost per hour \(`CostPerHour`\) for your real\-time endpoint in US dollars, the estimated cost per inference \(`CostPerInference`\) in US dollars for your real\-time endpoint, the expected maximum number of `InvokeEndpoint` requests per minute sent to the endpoint \(`MaxInvocations`\), and the model latency \(`ModelLatency`\), which is the interval of time \(in milliseconds\) that your model took to respond to SageMaker\. The model latency includes the local communication times taken to send the request and to fetch the response from the container of a model and the time taken to complete the inference in the container\.

------
#### [ AWS CLI ]

Once an instance recommendation is complete, you can use `describe-inference-recommendations-job` to get the job details and recommended instance types\. Provide the job name that you used when you created the instance recommendation job\.

```
aws sagemaker describe-inference-recommendations-job\
    --job-name <job-name>\
    --region <aws-region>
```

The JSON response similar should resemble the following:

```
{
    'JobName': 'job-name', 
    'JobDescription': 'job-description', 
    'JobType': 'Default', 
    'JobArn': 'arn:aws:sagemaker:region:account-id:inference-recommendations-job/resource-id', 
    'Status': 'COMPLETED', 
    'CreationTime': datetime.datetime(2021, 10, 26, 20, 4, 57, 627000, tzinfo=tzlocal()), 
    'LastModifiedTime': datetime.datetime(2021, 10, 26, 20, 25, 1, 997000, tzinfo=tzlocal()), 
    'InputConfig': {
                'ModelPackageVersionArn': 'arn:aws:sagemaker:region:account-id:model-package/resource-id', 
                'JobDurationInSeconds': 0
                }, 
    'InferenceRecommendations': [{
            'Metrics': {
                'CostPerHour': 0.20399999618530273, 
                'CostPerInference': 5.246913588052848e-06, 
                'MaximumInvocations': 648, 
                'ModelLatency': 263596
                }, 
            'EndpointConfiguration': {
                'EndpointName': 'endpoint-name', 
                'VariantName': 'variant-name', 
                'InstanceType': 'ml.c5.xlarge', 
                'InitialInstanceCount': 1
                }, 
            'ModelConfiguration': {
                'Compiled': False, 
                'EnvironmentParameters': []
                }
         }, 
         {
            'Metrics': {
                'CostPerHour': 0.11500000208616257, 
                'CostPerInference': 2.92620870823157e-06, 
                'MaximumInvocations': 655, 
                'ModelLatency': 826019
                }, 
            'EndpointConfiguration': {
                'EndpointName': 'endpoint-name', 
                'VariantName': 'variant-name', 
                'InstanceType': 'ml.c5d.large', 
                'InitialInstanceCount': 1
                }, 
            'ModelConfiguration': {
                'Compiled': False, 
                'EnvironmentParameters': []
                }
            }, 
            {
                'Metrics': {
                    'CostPerHour': 0.11500000208616257, 
                    'CostPerInference': 3.3625731248321244e-06, 
                    'MaximumInvocations': 570, 
                    'ModelLatency': 1085446
                    }, 
                'EndpointConfiguration': {
                    'EndpointName': 'endpoint-name', 
                    'VariantName': 'variant-name', 
                    'InstanceType': 'ml.m5.large', 
                    'InitialInstanceCount': 1
                    }, 
                'ModelConfiguration': {
                    'Compiled': False, 
                    'EnvironmentParameters': []
                    }
            }], 
    'ResponseMetadata': {
        'RequestId': 'request-id', 
        'HTTPStatusCode': 200, 
        'HTTPHeaders': {
            'x-amzn-requestid': 'x-amzn-requestid', 
            'content-type': 'content-type', 
            'content-length': '1685', 
            'date': 'Tue, 26 Oct 2021 20:31:10 GMT'
            }, 
        'RetryAttempts': 0
        }
}
```

The first few lines provide information about the instance recommendation job itself\. This includes the job name, role ARN, creation, and deletion time\. 

The `InferenceRecommendations` dictionary contains a list of Inference Recommender instance recommendations\.

The `EndpointConfiguration` nested dictionary contains the instance type \(`InstanceType`\) recommendation along with the endpoint and variant name \(a deployed AWS machine learning model\) used during the recommendation job\. You can use the endpoint and variant name for monitoring in Amazon CloudWatch Events\. See [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md) for more information\.

The `Metrics` nested dictionary contains information about the estimated cost per hour \(`CostPerHour`\) for your real\-time endpoint in US dollars, the estimated cost per inference \(`CostPerInference`\) in US dollars for your real\-time endpoint, the expected maximum number of `InvokeEndpoint` requests per minute sent to the endpoint \(`MaxInvocations`\), and the model latency \(`ModelLatency`\), which is the interval of time \(in milliseconds\) that your model took to respond to SageMaker\. The model latency includes the local communication times taken to send the request and to fetch the response from the container of a model and the time taken to complete the inference in the container\.

------
#### [ Amazon SageMaker Studio ]

The instance recommendations populate in a new **Inference recommendations** tab within Studio\. It can take up to 45 minutes for the results to show up\. This tab contains **Results** and **Details** column headings\.

The **Details** column provides information about the instance recommendation job, such as the name of the instance recommendation, when the job was created \(**Creation time**\), and more\. It also provides **Settings** information, such as the maximum number of invocations that occurred per minute and information about the Amazon Resource Names used\.

The **Results** column provides a ** Deployment goals** and **SageMaker recommendations** window in which you can adjust the order that the results are displayed based on deployment importance\. There are three dropdown menus that you can use to provide the level of importance of the **Cost**, **Latency**, and **Throughput** for your use case\. For each goal \(cost, latency, and throughput\), you can set the level of importance: **Lowest Importance**, **Low Importance**, **Moderate importance**, **High importance**, or **Highest importance**\. 

Based on your selections of importance for each goal, Inference Recommender displays its top recommendation in the **SageMaker recommendation** field on the right of the panel, along with the estimated cost per hour and inference request\. It also provides information about the expected model latency, maximum number of invocations, and the number of instances\.

In addition to the top recommendation displayed, you can also see the same information displayed for all instances that Inference Recommender tested in the ** All runs** section\.

------

## Stop Your Instance Endpoint Recommendation<a name="instance-recommendation-stop"></a>

Stop your Inference Recommender instance recommendation jobs programmatically with the `StopInferenceRecommendationsJob` API or with Studio\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Specify the name of the instance recommendation job for the `JobName` field:

```
sagemaker_client.stop_inference_recommendations_job(
                                    JobName='<INSERT>'
                                    )
```

------
#### [ AWS CLI ]

Specify the job name of the instance recommendation job for the `job-name` flag:

```
aws sagemaker stop-inference-recommendations-job --job-name <job-name>
```

------
#### [ Amazon SageMaker Studio ]

Close the tab in which you initiated the instance recommendation to stop your Inference Recommender instance recommendation\.

------