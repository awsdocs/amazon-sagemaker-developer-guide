# Run a custom load test<a name="inference-recommender-load-test"></a>

Amazon SageMaker Inference Recommender load tests conduct extensive benchmarks based on production requirements for latency and throughput, custom traffic patterns, and instances \(up to 10\) that you select\.

The following sections demonstrate how to create, describe, and stop a load test programmaticially using AWS SDK for Python \(Boto3\) and the AWS CLI, and interactively using Amazon SageMaker Studio\.

## Create a Load Test Job<a name="load-test-create"></a>

Create a load test programmaticially using AWS SDK for Python \(Boto3\), with the AWS CLI, or interactively using Studio\. As with Inference Recommender instance recommendations, specify a job name for your load test, an AWS IAM role ARN, an input configuration, and your model package ARN when you registered your model with the model registry\. Load tests require that you also specify a traffic pattern and stopping conditions\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Use the `CreateInferenceRecommendationsJob` API to create an Inference Recommender load test\. Specify `Advanced` for the `JobType` field and provide: 
+ A job name for your load test \(`JobName`\)\. The job name must be unique within your AWS Region and within your AWS account\.
+ The Amazon Resource Name \(ARN\) of an IAM role that enables Inference Recommender to perform tasks on your behalf\. Define this for the `RoleArn` field\.
+ A traffic pattern of the load test \(`TrafficPattern`\)\.
+ An endpoint configuration dictionary \(`InputConfig`\) where you specify an AWS instance type against which to run benchmarks\.

```
# Create a low-level SageMaker service client.
import boto3
aws_region=<INSERT>
sagemaker_client=boto3.client('sagemaker', region=aws_region) 
                
# Provide a name to your recommendation based on load testing
load_test_job_name="<INSERT>"

# Provide the name of the sagemaker instance type
instance_type="<INSERT>"

# Provide the IAM Role that gives SageMaker permission to access AWS services 
role_arn='arn:aws:iam::<account>:role/*'

# Provide your model package ARN that was created when you registered your 
# model with Model Registry
model_package_arn='arn:aws:sagemaker:<region>:<account>:role/*'

sagemaker_client.create_inference_recommendations_job(
                        JobName=load_test_job_name,
                        JobType="Advanced",
                        RoleArn=role_arn,
                        InputConfig={
                            'ModelPackageVersionArn': model_package_arn,
                            "JobDurationInSeconds": 7200,
                            'TrafficPattern' : {
                                'TrafficType': 'PHASES',
                                'Phases': [
                                    {
                                        'InitialNumberOfUsers': 1,
                                        'SpawnRate': 60,
                                        'DurationInSeconds': 300
                                    },
                                    {
                                        'InitialNumberOfUsers': 5,
                                        'SpawnRate': 1,
                                        'DurationInSeconds': 300
                                    }
                                ]
                            },
                            'ResourceLimits': {
                                        'MaxNumberOfTests': 10,
                                        'MaxParallelOfTests': 3
                                },
                            "EndpointConfigurations" : [{
                                        'InstanceType': 'ml.c5.xlarge'
                                    },
                                    {
                                        'InstanceType': 'ml.m5.xlarge'
                                    },
                                    {
                                        'InstanceType': 'ml.r5.xlarge'
                                    }]
                        },
                        StoppingConditions={
                            'MaxInvocations': 1000,
                            'ModelLatencyThresholds':[{
                                'Percentile': 'P95', 
                                'ValueInMilliseconds': 100
                            }]
                        }
                )
```

See the [Amazon SageMaker API Reference Guide](https://docs.aws.amazon.com/sagemaker/latest/APIReference/Welcome.html) for a full list of optional and required arguments you can pass to `CreateInferenceRecommendationsJob`\.

------
#### [ AWS CLI ]

Use the `create-inference-recommendations-job` API to create an Inference Recommender load test\. Specify `Advanced` for the `JobType` field and provide: 
+ A job name for your load test \(`job-name`\)\. The job name must be unique within your AWS region and within your AWS account\.
+ The Amazon Resource Name \(ARN\) of an IAM role that enables Inference Recommender to perform tasks on your behalf\. Define this for the `role-arn` field\.
+ A traffic pattern of the load test \(`TrafficPattern`\)\.
+ An endpoint configuration dictionary \(`input-config`\) where you specify an AWS instance type for running benchmarks against\.

```
aws sagemaker create-inference-recommendations-job\
    --region <region>\
    --job-name <job-name>\
    --job-type ADVANCED\
    --role-arn arn:aws:iam::<account>:role/*\
    --input-config "{
        \"ModelPackageVersionArn\": \"arn:aws:sagemaker:<region>:<account>:role/*\",
        \"JobDurationInSeconds\": 7200,                                
        \"TrafficPattern\" : {
                \"TrafficType\": \"PHASES\",
                \"Phases\": [
                    {
                        \"InitialNumberOfUsers\": 1,
                        \"SpawnRate\": 60,
                        \"DurationInSeconds\": 300
                    }
                ]
            },
            \"ResourceLimit\": {
                \"MaxNumberOfTests\": 10,
                \"MaxParallelOfTests\": 3
            },
            \"EndpointConfigurations\" : [
                {
                    \"InstanceType\": \"ml.c5.xlarge\"
                },
                {
                    \"InstanceType": "ml.m5.xlarge"
                },
                {
                    'InstanceType': 'ml.r5.xlarge'
                }
            ]
        }"
    --stopping-conditions "{
        \"MaxInvocations\": 1000,
        \"ModelLatencyThresholds\":[
                {
                    \"Percentile\": \"P95\", 
                    \"ValueInMilliseconds\": 100
                }
        ]
    }"
```

------
#### [ Amazon SageMaker Studio ]

Create a load test with Studio\.

1. In the left sidebar of Studio, choose the **Components and registries** icon\.

1. Select **Inference Recommender Jobs** from the dropdown list\. A new tab titled **Create inference recommender job** opens in the right pane\. 

1. Select the name of your model group from the dropdown **Model group** field\. The list includes all the model groups registered with the model registry in your account, including models registered outside of Studio\.

1. Select a model version from the dropdown **Model version** field\.

1. Choose **Continue**\.

1. Provide a name for the job in the **Name** field\.

1. \(Optional\) Provide a description of your job in the **Description** field\.

1. Choose an IAM role that grants Inference Recommender permission to access AWS services\. You can create a role and attach the `AmazonSageMakerFullAccess` IAM managed policy to accomplish this, or you can let Studio create a role for you\.

1. Choose **Stopping Conditions** to expand the available input fields\. Provide a set of conditions for stopping a deployment recommendation\. 

   1. Specify the maximum number of requests per minute expected for the endpoint in the **Max Invocations Per Minute** field\.

   1. Specify the model latency threshold in microseconds in the **Model Latency Threshold** field\. The **Model Latency Threshold** depicts the interval of time taken by a model to respond as viewed from Inference Recommender\. The interval includes the local communication time taken to send the request and to fetch the response from the model container and the time taken to complete the inference in the container\.

1. Choose **Traffic Pattern** to expand the available input fields\.

   1. Set the initial number of virtual users by specifying an integer in the **Initial Number of Users** field\.

   1. Provide an integer number for the **Spawn Rate** field\. The spawn rate sets the number of users created per second\.

   1. Set the duration for the phase in seconds by specifying an integer in the **Duration** field\.

   1. \(Optional\) Add additional traffic patterns\. To do so, choose **Add**\.

1. Choose the **Additional** setting to reveal the **Max test duration** field\. Specify, in seconds, the maximum time a test can take during a job\. New jobs are not scheduled after the defined duration\. This helps ensure jobs that are in progress are not stopped and that you only view completed jobs\.

1. Choose **Continue**\.

1. Choose **Selected Instances**\.

1. In the **Instances for benchmarking** field, choose **Add instances to test**\. Select up to 10 instances for Inference Recommender to use for load testing\.

1. Choose **Additional settings**\.

   1. Provide an integer that sets an upper limit on the number of tests a job can make for the **Max number of tests field**\. Note that each endpoint configuration results in a new load test\.

   1. Provide an integer for the **Max parallel** test field\. This setting defines an upper limit on the number of load tests that can run in parallel\.

1. Choose **Submit**\.

   The load test can take up to 2 hours\.
**Warning**  
Do not close this tab\. If you close this tab, you cancel the Inference Recommender load test job\.

------

## Get Your Load Test Results<a name="load-test-describe"></a>

You can collect metrics across all load tests once the load tests are done programmatically with AWS SDK for Python \(Boto3\), the AWS CLI, or Studio\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Collect metrics with the `DescribeInferenceRecommendationsJob` API\. Specify the job name of the load test for the `JobName` field:

```
load_test_response = sagemaker_client.describe_inference_recommendations_job(
                                                        job_name=load_test_job_name
                                                        )
```

Print the response object\.

```
load_test_response['Status']
```

This returns a JSON response similar to the following:

```
{
    'JobName': 'job-name', 
    'JobDescription': 'job-description', 
    'JobType': 'Advanced', 
    'JobArn': 'arn:aws:sagemaker:region:account-id:inference-recommendations-job/resource-id', 
    'Status': 'COMPLETED', 
    'CreationTime': datetime.datetime(2021, 10, 26, 19, 38, 30, 957000, tzinfo=tzlocal()), 
    'LastModifiedTime': datetime.datetime(2021, 10, 26, 19, 46, 31, 399000, tzinfo=tzlocal()), 
    'InputConfig': {
        'ModelPackageVersionArn': 'arn:aws:sagemaker:region:account-id:model-package/resource-id', 
        'JobDurationInSeconds': 7200, 
        'TrafficPattern': {
            'TrafficType': 'PHASES'
            }, 
        'ResourceLimit': {
            'MaxNumberOfTests': 100, 
            'MaxParallelOfTests': 100
            }, 
        'EndpointConfigurations': [{
            'InstanceType': 'ml.c5d.xlarge'
            }]
        }, 
    'StoppingConditions': {
        'MaxInvocations': 1000, 
        'ModelLatencyThresholds': [{
            'Percentile': 'P95', 
            'ValueInMilliseconds': 100}
            ]}, 
    'InferenceRecommendations': [{
        'Metrics': {
            'CostPerHour': 0.6899999976158142, 
            'CostPerInference': 1.0332434612791985e-05, 
            'MaximumInvocations': 1113, 
            'ModelLatency': 100000
            }, 
    'EndpointConfiguration': {
        'EndpointName': 'endpoint-name', 
        'VariantName': 'variant-name', 
        'InstanceType': 'ml.c5d.xlarge', 
        'InitialInstanceCount': 3
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
            'content-length': '1199', 
            'date': 'Tue, 26 Oct 2021 19:57:42 GMT'
            }, 
        'RetryAttempts': 0}
    }
```

The first few lines provide information about the load test job itself\. This includes the job name, role ARN, creation, and deletion time\. 

The `InferenceRecommendations` dictionary contains a list of Inference Recommender instance recommendations\.

The `EndpointConfiguration` nested dictionary contains the instance type \(`InstanceType`\) recommendation along with the endpoint and variant name \(a deployed AWS machine learning model\) used during the recommendation job\. You can use the endpoint and variant name for monitoring in Amazon CloudWatch Events\. See [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md) for more information\.

The `Metrics` nested dictionary contains information about the estimated cost per hour \(`CostPerHour`\) for your real\-time endpoint in US dollars, the estimated cost per inference \(`CostPerInference`\) for your real\-time endpoint, the maximum number of `InvokeEndpoint` requests sent to the endpoint, and the model latency \(`ModelLatency`\), which is the interval of time \(in microseconds\) that your model took to respond to SageMaker\. The model latency includes the local communication times taken to send the request and to fetch the response from the model container and the time taken to complete the inference in the container\.

------
#### [ AWS CLI ]

Collect metrics with the `describe-inference-recommendations-job` API\. Specify the job name of the load test for the `job-name` flag:

```
aws sagemaker describe-inference-recommendations-job --job-name <job-name>
```

This returns a response similar to the following:

```
{
    'JobName': 'job-name', 
    'JobDescription': 'job-description', 
    'JobType': 'Advanced', 
    'JobArn': 'arn:aws:sagemaker:region:account-id:inference-recommendations-job/resource-id', 
    'Status': 'COMPLETED', 
    'CreationTime': datetime.datetime(2021, 10, 26, 19, 38, 30, 957000, tzinfo=tzlocal()), 
    'LastModifiedTime': datetime.datetime(2021, 10, 26, 19, 46, 31, 399000, tzinfo=tzlocal()), 
    'InputConfig': {
        'ModelPackageVersionArn': 'arn:aws:sagemaker:region:account-id:model-package/resource-id', 
        'JobDurationInSeconds': 7200, 
        'TrafficPattern': {
            'TrafficType': 'PHASES'
            }, 
        'ResourceLimit': {
            'MaxNumberOfTests': 100, 
            'MaxParallelOfTests': 100
            }, 
        'EndpointConfigurations': [{
            'InstanceType': 'ml.c5d.xlarge'
            }]
        }, 
    'StoppingConditions': {
        'MaxInvocations': 1000, 
        'ModelLatencyThresholds': [{
            'Percentile': 'P95', 
            'ValueInMilliseconds': 100
            }]
        }, 
    'InferenceRecommendations': [{
        'Metrics': {
        'CostPerHour': 0.6899999976158142, 
        'CostPerInference': 1.0332434612791985e-05, 
        'MaximumInvocations': 1113, 
        'ModelLatency': 100000
        }, 
        'EndpointConfiguration': {
            'EndpointName': 'endpoint-name', 
            'VariantName': 'variant-name', 
            'InstanceType': 'ml.c5d.xlarge', 
            'InitialInstanceCount': 3
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
            'content-length': '1199', 
            'date': 'Tue, 26 Oct 2021 19:57:42 GMT'
            }, 
        'RetryAttempts': 0
        }
    }
```

The first few lines provide information about the load test job itself\. This includes the job name, role ARN, creation, and deletion time\. 

The `InferenceRecommendations` dictionary contains a list of Inference Recommender instance recommendations\.

The `EndpointConfiguration` nested dictionary contains the instance type \(`InstanceType`\) recommendation along with the endpoint and variant name \(a deployed AWS machine learning model\) used during the recommendation job\. You can use the endpoint and variant name for monitoring in Amazon CloudWatch Events\. See [Monitor Amazon SageMaker with Amazon CloudWatch](monitoring-cloudwatch.md) for more information\.

The `Metrics` nested dictionary contains information about the estimated cost per hour \(`CostPerHour`\) for your real\-time endpoint in US dollars, the estimated cost per inference \(`CostPerInference`\) for your real\-time endpoint, the maximum number of `InvokeEndpoint` requests sent to the endpoint, and the model latency \(`ModelLatency`\), which is the interval of time \(in microseconds\) that your model took to respond to SageMaker\. The model latency includes the local communication times taken to send the request and to fetch the response from the model container and the time taken to complete the inference in the container\.

------
#### [ Amazon SageMaker Studio ]

The recommendations populate in a new tab called **Inference recommendations** within Studio\. It can take up to 2 hours for the results to show up\. This tab contains **Results** and **Details** columns\.

The **Details** column provides information about the load test job, such as the name given to the load test job, when the job was created \(**Creation time**\), and more\. It also contains **Settings** information, such as the maximum number of invocation that occurred per minute and information about the Amazon Resource Names used\.

The **Results** column provides ** Deployment goals** and **SageMaker recommendations** windows in which you can adjust the order in which results are displayed based on deployment importance\. There are three dropdown menus in which you can provide the level of importance of the **Cost**, **Latency**, and **Throughput** for your use case\. For each goal \(cost, latency, and throughput\), you can set the level of importance: **Lowest Importance**, **Low Importance**, **Moderate importance**, **High importance**, or **Highest importance**\. 

Based on your selections of importance for each goal, Inference Recommender displays its top recommendation in the **SageMaker recommendation** field on the right of the panel, along with the estimated cost per hour and inference request\. It also provides Information about the expected model latency, maximum number of invocations, and the number of instances\.

In addition to the top recommendation displayed, you can also see the same information displayed for all instances that Inference Recommender tested in the ** All runs** section\.

------

## Stop Your Load Test<a name="load-test-stop"></a>

Stop your load test jobs programmatically with the `StopInferenceRecommendationsJob` API or with Studio\.

------
#### [ AWS SDK for Python \(Boto3\) ]

Specify the job name of the load test for the `JobName` field:

```
sagemaker_client.stop_inference_recommendations_job(
                                    job_name='<INSERT>'
                                    )
```

------
#### [ AWS CLI ]

Specify the job name of the load test for the `job-name` flag:

```
aws sagemaker stop-inference-recommendations-job --job-name <job-name>
```

------
#### [ Amazon SageMaker Studio ]

Close the tab where you initiated your custom load job to stop your Inference Recommender load test\.

------