# AWS Boto3<a name="debugger-built-in-rules-api.Boto3"></a>

Amazon SageMaker Debugger built\-in rules can be configured for a training job using the [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job) function of the AWS Boto3 SageMaker client\. You need to specify the right image URI in the `RuleEvaluatorImage` parameter, and the following examples walk you through how to set up the request body for the [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job) function\.

The following code shows a complete example of how to configure Debugger for the `create_training_job()` request body and start a training job in `us-west-2`, assuming that a training script `entry_point/train.py` is prepared using TensorFlow\. To find an end\-to\-end example notebook, see [Profiling TensorFlow Multi GPU Multi Node Training Job with Amazon SageMaker Debugger \(Boto3\)](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-debugger/tensorflow_profiling/tf-resnet-profiling-multi-gpu-multi-node-boto3.html)\.

**Note**  
Ensure that you use the correct Docker container images\. To find available AWS Deep Learning Container images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\.

```
import sagemaker, boto3
import datetime, tarfile

# Start setting up a SageMaker session and a Boto3 SageMaker client
session = sagemaker.Session()
region = session.boto_region_name
bucket = session.default_bucket()

# Upload a training script to a default Amazon S3 bucket of the current SageMaker session
source = 'source.tar.gz'
project = 'debugger-boto3-test'

tar = tarfile.open(source, 'w:gz')
tar.add ('entry_point/train.py') # Specify the directory and name of your training script
tar.close()

s3 = boto3.client('s3')
s3.upload_file(source, bucket, project+'/'+source)

# Set up a Boto3 session client for SageMaker
sm = boto3.Session(region_name=region).client("sagemaker")

# Start a training job
sm.create_training_job(
    TrainingJobName='debugger-boto3-'+datetime.datetime.now().strftime('%Y-%m-%d-%H-%M-%S'),
    HyperParameters={
        'sagemaker_submit_directory': 's3://'+bucket+'/'+project+'/'+source,
        'sagemaker_program': '/entry_point/train.py' # training scrip file location and name under the sagemaker_submit_directory
    },
    AlgorithmSpecification={
        # Specify a training Docker container image URI (Deep Learning Container or your own training container) to TrainingImage.
        'TrainingImage': '763104351884.dkr.ecr.us-west-2.amazonaws.com/tensorflow-training:2.4.1-gpu-py37-cu110-ubuntu18.04',
        'TrainingInputMode': 'File',
        'EnableSageMakerMetricsTimeSeries': False
    },
    RoleArn='arn:aws:iam::111122223333:role/service-role/AmazonSageMaker-ExecutionRole-20201014T161125',
    OutputDataConfig={'S3OutputPath': 's3://'+bucket+'/'+project+'/output'},
    ResourceConfig={
        'InstanceType': 'ml.p3.8xlarge',
        'InstanceCount': 1,
        'VolumeSizeInGB': 30
    },
    StoppingCondition={
        'MaxRuntimeInSeconds': 86400
    },
    DebugHookConfig={
        'S3OutputPath': 's3://'+bucket+'/'+project+'/debug-output',
        'CollectionConfigurations': [
            {
                'CollectionName': 'losses',
                'CollectionParameters' : {
                    'train.save_interval': '500',
                    'eval.save_interval': '50'
                }
            }
        ]
    },
    DebugRuleConfigurations=[
        {
            'RuleConfigurationName': 'LossNotDecreasing',
            'RuleEvaluatorImage': '895741380848.dkr.ecr.us-west-2.amazonaws.com/sagemaker-debugger-rules:latest',
            'RuleParameters': {'rule_to_invoke': 'LossNotDecreasing'}
        }
    ],
    ProfilerConfig={
        'S3OutputPath': 's3://'+bucket+'/'+project+'/profiler-output',
        'ProfilingIntervalInMilliseconds': 500,
        'ProfilingParameters': {
            'DataloaderProfilingConfig': '{"StartStep": 5, "NumSteps": 3, "MetricsRegex": ".*", }',
            'DetailedProfilingConfig': '{"StartStep": 5, "NumSteps": 3, }',
            'PythonProfilingConfig': '{"StartStep": 5, "NumSteps": 3, "ProfilerName": "cprofile", "cProfileTimer": "total_time"}',
            'LocalPath': '/opt/ml/output/profiler/' # Optional. Local path for profiling outputs
        }
    },
    ProfilerRuleConfigurations=[
        {
            'RuleConfigurationName': 'ProfilerReport',
            'RuleEvaluatorImage': '895741380848.dkr.ecr.us-west-2.amazonaws.com/sagemaker-debugger-rules:latest',
            'RuleParameters': {'rule_to_invoke': 'ProfilerReport'}
        }
    ]
)
```

## To configure a Debugger rule for debugging model parameters<a name="debugger-built-in-rules-api-debug.Boto3"></a>

The following code samples show how to configure a built\-in `VanishingGradient` rule using this SageMaker API\. 

**To enable Debugger to collect output tensors**

Specify the Debugger hook configuration as follows:

```
DebugHookConfig={
    'S3OutputPath': 's3://<default-bucket>/<training-job-name>/debug-output',
    'CollectionConfigurations': [
        {
            'CollectionName': 'gradients',
            'CollectionParameters' : {
                'train.save_interval': '500',
                'eval.save_interval': '50'
            }
        }
    ]
}
```

This will make the training job save a tensor collection, `gradients`, every `save_interval` of 500 steps\. To find available `CollectionName` values, see [Debugger Built\-in Collections](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md#built-in-collections) in the *SMDebug client library documentation*\. To find available `CollectionParameters` parameter keys and values, see the [https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.CollectionConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.CollectionConfig) class in the *SageMaker Python SDK documentation*\.

**To enable Debugger rules for debugging the output tensors**

The following `DebugRuleConfigurations` API example shows how to run the built\-in `VanishingGradient` rule on the saved `gradients` collection\.

```
DebugRuleConfigurations=[
    {
        'RuleConfigurationName': 'VanishingGradient',
        'RuleEvaluatorImage': '895741380848.dkr.ecr.us-west-2.amazonaws.com/sagemaker-debugger-rules:latest',
        'RuleParameters': {
            'rule_to_invoke': 'VanishingGradient',
            'threshold': '20.0'
        }
    }
]
```

With a configuration like the one in this sample, Debugger starts a rule evaluation job for your training job using the `VanishingGradient` rule on the collection of `gradients` tensor\. To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. To find the key\-value pairs for `RuleParameters`, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

## To configure a Debugger built\-in rule for profiling system and framework metrics<a name="debugger-built-in-rules-api-profile.Boto3"></a>

The following example code shows how to specify the ProfilerConfig API operation to enable collecting system and framework metrics\.

**To enable Debugger profiling to collect system and framework metrics**

------
#### [ Target Step ]

```
ProfilerConfig={ 
    'S3OutputPath': 's3://<default-bucket>/<training-job-name>/profiler-output', # Optional. Path to an S3 bucket to save profiling outputs
    # Available values for ProfilingIntervalInMilliseconds: 100, 200, 500, 1000 (1 second), 5000 (5 seconds), and 60000 (1 minute) milliseconds.
    'ProfilingIntervalInMilliseconds': 500, 
    'ProfilingParameters': {
        'DataloaderProfilingConfig': '{
            "StartStep": 5, 
            "NumSteps": 3, 
            "MetricsRegex": ".*"
        }',
        'DetailedProfilingConfig': '{
            "StartStep": 5, 
            "NumSteps": 3 
        }',
        'PythonProfilingConfig': '{
            "StartStep": 5, 
            "NumSteps": 3, 
            "ProfilerName": "cprofile",  # Available options: cprofile, pyinstrument
            "cProfileTimer": "total_time"  # Include only when using cprofile. Available options: cpu, off_cpu, total_time
        }',
        'LocalPath': '/opt/ml/output/profiler/' # Optional. Local path for profiling outputs
    }
}
```

------
#### [ Target Time Duration ]

```
ProfilerConfig={ 
    'S3OutputPath': 's3://<default-bucket>/<training-job-name>/profiler-output', # Optional. Path to an S3 bucket to save profiling outputs
    # Available values for ProfilingIntervalInMilliseconds: 100, 200, 500, 1000 (1 second), 5000 (5 seconds), and 60000 (1 minute) milliseconds.
    'ProfilingIntervalInMilliseconds': 500,
    'ProfilingParameters': {
        'DataloaderProfilingConfig': '{
            "StartTimeInSecSinceEpoch": 12345567789, 
            "DurationInSeconds": 10, 
            "MetricsRegex": ".*"
        }',
        'DetailedProfilingConfig': '{
            "StartTimeInSecSinceEpoch": 12345567789, 
            "DurationInSeconds": 10
        }',
        'PythonProfilingConfig': '{
            "StartTimeInSecSinceEpoch": 12345567789, 
            "DurationInSeconds": 10, 
            "ProfilerName": "cprofile",  # Available options: cprofile, pyinstrument
            "cProfileTimer": "total_time"  # Include only when using cprofile. Available options: cpu, off_cpu, total_time
        }',
        'LocalPath': '/opt/ml/output/profiler/' # Optional. Local path for profiling outputs
    }
}
```

------

**To enable Debugger rules for profiling the metrics**

The following example code shows how to configure the `ProfilerReport` rule\.

```
ProfilerRuleConfigurations=[ 
    {
        'RuleConfigurationName': 'ProfilerReport',
        'RuleEvaluatorImage': '895741380848.dkr.ecr.us-west-2.amazonaws.com/sagemaker-debugger-rules:latest',
        'RuleParameters': {
            'rule_to_invoke': 'ProfilerReport',
            'CPUBottleneck_cpu_threshold': '90',
            'IOBottleneck_threshold': '90'
        }
    }
]
```

To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. To find the key\-value pairs for `RuleParameters`, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

## Update Debugger Profiling Configuration Using the `UpdateTrainingJob` API Operation<a name="debugger-updatetrainingjob-api.Boto3"></a>

Debugger profiling configuration can be updated while your training job is running by using the [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.update_training_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.update_training_job) function of the AWS Boto3 SageMaker client\. Configure new [ProfilerConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerConfig.html) and [ProfilerRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerRuleConfiguration.html) objects, and specify the training job name to the `TrainingJobName` parameter\.

```
ProfilerConfig={ 
    'DisableProfiler': boolean,
    'ProfilingIntervalInMilliseconds': number,
    'ProfilingParameters': { 
        'string' : 'string' 
    }
},
ProfilerRuleConfigurations=[ 
    { 
        'RuleConfigurationName': 'string',
        'RuleEvaluatorImage': 'string',
        'RuleParameters': { 
            'string' : 'string' 
        }
    }
],
TrainingJobName='your-training-job-name-YYYY-MM-DD-HH-MM-SS-SSS'
```

## Add Debugger Custom Rule Configuration to the CreateTrainingJob API Operation<a name="debugger-custom-rules-api.Boto3"></a>

A custom rule can be configured for a training job using the [ DebugHookConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html) and [ DebugRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html) objects using the AWS Boto3 SageMaker client's [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job) function\. The following code sample shows how to configure a custom `ImproperActivation` rule written with the *smdebug* library using this SageMaker API operation\. This example assumes that you’ve written the custom rule in *custom\_rules\.py* file and uploaded it to an Amazon S3 bucket\. The example provides pre\-built Docker images that you can use to run your custom rules\. These are listed at [Amazon SageMaker Debugger Registry URLs for Custom Rule Evaluators](debugger-docker-images-rules.md#debuger-custom-rule-registry-ids)\. You specify the URL registry address for the pre\-built Docker image in the `RuleEvaluatorImage` parameter\.

```
DebugHookConfig={
    'S3OutputPath': 's3://<default-bucket>/<training-job-name>/debug-output',
    'CollectionConfigurations': [
        {
            'CollectionName': 'relu_activations',
            'CollectionParameters': {
                'include_regex': 'relu',
                'save_interval': '500',
                'end_step': '5000'
            }
        }
    ]
},
DebugRulesConfigurations=[
    {
        'RuleConfigurationName': 'improper_activation_job',
        'RuleEvaluatorImage': '552407032007.dkr.ecr.ap-south-1.amazonaws.com/sagemaker-debugger-rule-evaluator:latest',
        'InstanceType': 'ml.c4.xlarge',
        'VolumeSizeInGB': 400,
        'RuleParameters': {
           'source_s3_uri': 's3://bucket/custom_rules.py',
           'rule_to_invoke': 'ImproperActivation',
           'collection_names': 'relu_activations'
        }
    }
]
```

To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. To find the key\-value pairs for `RuleParameters`, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.