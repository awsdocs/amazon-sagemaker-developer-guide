# JSON \(AWS CLI\)<a name="debugger-built-in-rules-api.CLI"></a>

Amazon SageMaker Debugger built\-in rules can be configured for a training job using the [DebugHookConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html), [DebugRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html), [ProfilerConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerConfig.html), and [ProfilerRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerRuleConfiguration.html) objects through the SageMaker [CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API operation\. You need to specify the right image URI in the `RuleEvaluatorImage` parameter, and the following examples walk you through how to set up the JSON strings to request [CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)\.

The following code shows a complete JSON template to run a training job with required settings and Debugger configurations\. Save the template as a JSON file in your working directory and run the training job using AWS CLI\. For example, save the following code as `debugger-training-job-cli.json`\.

**Note**  
Ensure that you use the correct Docker container images\. To find AWS Deep Learning Container images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\.

```
{
   "TrainingJobName": "debugger-aws-cli-test",
   "RoleArn": "arn:aws:iam::111122223333:role/service-role/AmazonSageMaker-ExecutionRole-YYYYMMDDT123456",
   "AlgorithmSpecification": {
      // Specify a training Docker container image URI (Deep Learning Container or your own training container) to TrainingImage.
      "TrainingImage": "763104351884.dkr.ecr.us-west-2.amazonaws.com/tensorflow-training:2.4.1-gpu-py37-cu110-ubuntu18.04",
      "TrainingInputMode": "File",
      "EnableSageMakerMetricsTimeSeries": false
   },
   "HyperParameters": {
      "sagemaker_program": "entry_point/tf-hvd-train.py",
      "sagemaker_submit_directory": "s3://sagemaker-us-west-2-111122223333/debugger-boto3-profiling-test/source.tar.gz"
   },
   "OutputDataConfig": { 
      "S3OutputPath": "s3://sagemaker-us-west-2-111122223333/debugger-aws-cli-test/output"
   },
   "DebugHookConfig": { 
      "S3OutputPath": "s3://sagemaker-us-west-2-111122223333/debugger-aws-cli-test/debug-output",
      "CollectionConfigurations": [
         {
            "CollectionName": "losses",
            "CollectionParameters" : {
                "train.save_interval": "50"
            }
         }
      ]
   },
   "DebugRuleConfigurations": [ 
      { 
         "RuleConfigurationName": "LossNotDecreasing",
         "RuleEvaluatorImage": "895741380848.dkr.ecr.us-west-2.amazonaws.com/sagemaker-debugger-rules:latest",
         "RuleParameters": {"rule_to_invoke": "LossNotDecreasing"}
      }
   ],
   "ProfilerConfig": { 
      "S3OutputPath": "s3://sagemaker-us-west-2-111122223333/debugger-aws-cli-test/profiler-output",
      "ProfilingIntervalInMilliseconds": 500,
      "ProfilingParameters": {
          "DataloaderProfilingConfig": "{\"StartStep\": 5, \"NumSteps\": 3, \"MetricsRegex\": \".*\", }",
          "DetailedProfilingConfig": "{\"StartStep\": 5, \"NumSteps\": 3, }",
          "PythonProfilingConfig": "{\"StartStep\": 5, \"NumSteps\": 3, \"ProfilerName\": \"cprofile\", \"cProfileTimer\": \"total_time\"}",
          "LocalPath": "/opt/ml/output/profiler/" 
      }
   },
   "ProfilerRuleConfigurations": [ 
      { 
         "RuleConfigurationName": "ProfilerReport",
         "RuleEvaluatorImage": "895741380848.dkr.ecr.us-west-2.amazonaws.com/sagemaker-debugger-rules:latest",
         "RuleParameters": {"rule_to_invoke": "ProfilerReport"}
      }
   ],
   "ResourceConfig": { 
      "InstanceType": "ml.p3.8xlarge",
      "InstanceCount": 1,
      "VolumeSizeInGB": 30
   },
   
   "StoppingCondition": { 
      "MaxRuntimeInSeconds": 86400
   }
}
```

After saving the JSON file, run the following command in your terminal\. \(Use `!` at the beginning of the line if you use a Jupyter notebook\.\)

```
aws sagemaker create-training-job --cli-input-json file://debugger-training-job-cli.json
```

## To configure a Debugger rule for debugging model parameters<a name="debugger-built-in-rules-api-debug.CLI"></a>

The following code samples show how to configure a built\-in `VanishingGradient` rule using this SageMaker API\. 

**To enable Debugger to collect output tensors**

Specify the Debugger hook configuration as follows:

```
"DebugHookConfig": {
    "S3OutputPath": "s3://<default-bucket>/<training-job-name>/debug-output",
    "CollectionConfigurations": [
        {
            "CollectionName": "gradients",
            "CollectionParameters" : {
                "save_interval": "500"
            }
        }
    ]
}
```

This will make the training job save the tensor collection, `gradients`, every `save_interval` of 500 steps\. To find available `CollectionName` values, see [Debugger Built\-in Collections](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md#built-in-collections) in the *SMDebug client library documentation*\. To find available `CollectionParameters` parameter keys and values, see the [https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.CollectionConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.CollectionConfig) class in the *SageMaker Python SDK documentation*\.

**To enable Debugger rules for debugging the output tensors**

The following `DebugRuleConfigurations` API example shows how to run the built\-in `VanishingGradient` rule on the saved `gradients` collection\.

```
"DebugRuleConfigurations": [
    {
        "RuleConfigurationName": "VanishingGradient",
        "RuleEvaluatorImage": "503895931360.dkr.ecr.us-east-1.amazonaws.com/sagemaker-debugger-rules:latest",
        "RuleParameters": {
            "rule_to_invoke": "VanishingGradient",
            "threshold": "20.0"
        }
    }
]
```

With a configuration like the one in this sample, Debugger starts a rule evaluation job for your training job using the `VanishingGradient` rule on the collection of `gradients` tensor\. To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. To find the key\-value pairs for `RuleParameters`, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

## To configure a Debugger built\-in rule for profiling system and framework metrics<a name="debugger-built-in-rules-api-profile.CLI"></a>

The following example code shows how to specify the ProfilerConfig API operation to enable collecting system and framework metrics\.

**To enable Debugger profiling to collect system and framework metrics**

------
#### [ Target Step ]

```
"ProfilerConfig": { 
    // Optional. Path to an S3 bucket to save profiling outputs
    "S3OutputPath": "s3://<default-bucket>/<training-job-name>/profiler-output", 
    // Available values for ProfilingIntervalInMilliseconds: 100, 200, 500, 1000 (1 second), 5000 (5 seconds), and 60000 (1 minute) milliseconds.
    "ProfilingIntervalInMilliseconds": 500, 
    "ProfilingParameters": {
        "DataloaderProfilingConfig": "{ \"StartStep\": 5, \"NumSteps\": 3, \"MetricsRegex\": \".*\" }",
        "DetailedProfilingConfig": "{ \"StartStep\": 5, \"NumSteps\": 3 }",
        // For PythonProfilingConfig,
        // available ProfilerName options: cProfile, Pyinstrument
        // available cProfileTimer options only when using cProfile: cpu, off_cpu, total_time
        "PythonProfilingConfig": "{ \"StartStep\": 5, \"NumSteps\": 3, \"ProfilerName\": \"cProfile\", \"cProfileTimer\": \"total_time\" }",
        // Optional. Local path for profiling outputs
        "LocalPath": "/opt/ml/output/profiler/" 
    }
}
```

------
#### [ Target Time Duration ]

```
"ProfilerConfig": { 
    // Optional. Path to an S3 bucket to save profiling outputs
    "S3OutputPath": "s3://<default-bucket>/<training-job-name>/profiler-output", 
    // Available values for ProfilingIntervalInMilliseconds: 100, 200, 500, 1000 (1 second), 5000 (5 seconds), and 60000 (1 minute) milliseconds.
    "ProfilingIntervalInMilliseconds": 500,
    "ProfilingParameters": {
        "DataloaderProfilingConfig": "{ \"StartTimeInSecSinceEpoch\": 12345567789, \"DurationInSeconds\": 10, \"MetricsRegex\": \".*\" }",
        "DetailedProfilingConfig": "{ \"StartTimeInSecSinceEpoch\": 12345567789, \"DurationInSeconds\": 10 }",
        // For PythonProfilingConfig,
        // available ProfilerName options: cProfile, Pyinstrument
        // available cProfileTimer options only when using cProfile: cpu, off_cpu, total_time
        "PythonProfilingConfig": "{ \"StartTimeInSecSinceEpoch\": 12345567789, \"DurationInSeconds\": 10, \"ProfilerName\": \"cProfile\", \"cProfileTimer\": \"total_time\" }",
        // Optional. Local path for profiling outputs
        "LocalPath": "/opt/ml/output/profiler/"  
    }
}
```

------

**To enable Debugger rules for profiling the metrics**

The following example code shows how to configure the `ProfilerReport` rule\.

```
"ProfilerRuleConfigurations": [ 
    {
        "RuleConfigurationName": "ProfilerReport",
        "RuleEvaluatorImage": "895741380848.dkr.ecr.us-west-2.amazonaws.com/sagemaker-debugger-rules:latest",
        "RuleParameters": {
            "rule_to_invoke": "ProfilerReport",
            "CPUBottleneck_cpu_threshold": "90",
            "IOBottleneck_threshold": "90"
        }
    }
]
```

To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. To find the key\-value pairs for `RuleParameters`, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

## Update Debugger Profiling Configuration Using the `UpdateTrainingJob` API Operation<a name="debugger-updatetrainingjob-api.CLI"></a>

Debugger profiling configuration can be updated while your training job is running by using the [UpdateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateTrainingJob.html) API operation\. Configure new [ProfilerConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerConfig.html) and [ProfilerRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerRuleConfiguration.html) objects, and specify the training job name to the `TrainingJobName` parameter\.

```
{
    "ProfilerConfig": { 
        "DisableProfiler": boolean,
        "ProfilingIntervalInMilliseconds": number,
        "ProfilingParameters": { 
            "string" : "string" 
        }
    },
    "ProfilerRuleConfigurations": [ 
        { 
            "RuleConfigurationName": "string",
            "RuleEvaluatorImage": "string",
            "RuleParameters": { 
                "string" : "string" 
            }
        }
    ],
    "TrainingJobName": "your-training-job-name-YYYY-MM-DD-HH-MM-SS-SSS"
}
```

## Add Debugger Custom Rule Configuration to the CreateTrainingJob API Operation<a name="debugger-custom-rules-api.CLI"></a>

A custom rule can be configured for a training job using the [ DebugHookConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html) and [ DebugRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html) objects in the [ CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API operation\. The following code sample shows how to configure a custom `ImproperActivation` rule written with the *smdebug* library using this SageMaker API operation\. This example assumes that you’ve written the custom rule in *custom\_rules\.py* file and uploaded it to an Amazon S3 bucket\. The example provides pre\-built Docker images that you can use to run your custom rules\. These are listed at [Amazon SageMaker Debugger Registry URLs for Custom Rule Evaluators](debugger-docker-images-rules.md#debuger-custom-rule-registry-ids)\. You specify the URL registry address for the pre\-built Docker image in the `RuleEvaluatorImage` parameter\.

```
"DebugHookConfig": {
    "S3OutputPath": "s3://<default-bucket>/<training-job-name>/debug-output",
    "CollectionConfigurations": [
        {
            "CollectionName": "relu_activations",
            "CollectionParameters": {
                "include_regex": "relu",
                "save_interval": "500",
                "end_step": "5000"
            }
        }
    ]
},
"DebugRulesConfigurations": [
    {
        "RuleConfigurationName": "improper_activation_job",
        "RuleEvaluatorImage": "552407032007.dkr.ecr.ap-south-1.amazonaws.com/sagemaker-debugger-rule-evaluator:latest",
        "InstanceType": "ml.c4.xlarge",
        "VolumeSizeInGB": 400,
        "RuleParameters": {
           "source_s3_uri": "s3://bucket/custom_rules.py",
           "rule_to_invoke": "ImproperActivation",
           "collection_names": "relu_activations"
        }
    }
]
```

To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. To find the key\-value pairs for `RuleParameters`, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.