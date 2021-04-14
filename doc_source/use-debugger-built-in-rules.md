# Configure Debugger Built\-in Rules<a name="use-debugger-built-in-rules"></a>

Amazon SageMaker Debugger rules analyze tensors emitted during the training of a model\. Debugger offers the `Rule` API operation that monitors training job progress and errors for the success of training your model\. For example, the rules can detect whether gradients are getting too large or too small, whether a model is overfitting or overtraining, and whether a training job does not decrease loss function and improve\. To see a full list of available built\-in rules, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

**Note**  
The built\-in rules are prepared in Amazon SageMaker processing containers and fully managed by SageMaker Debugger\. By default, Debugger initiates the [ProfilerReport](debugger-built-in-rules.md#profiler-report) rule for all SageMaker training jobs, without any Debugger\-specific rule parameter specified to the SageMaker estimators\. The ProfilerReport rule invokes all of the following built\-in rules for monitoring system bottlenecks and profiling framework metrics:   
`BatchSize`
`CPUBottleneck`
`GPUMemoryIncrease`
`IOBottleneck`
`LoadBalancing`
`LowGPUUtilization`
`OverallSystemUsage`
`MaxInitializationTime`
`OverallFrameworkMetrics`
`StepOutlier`
Debugger saves the profiling report in a default S3 bucket\. The format of the default S3 bucket URI is `s3://sagemaker-<region>-<12digit_account_id>/<training-job-name>/rule-output/`\. For more information about how to download the profiling report, see [SageMaker Debugger Profiling Report](debugger-profiling-report.md)\. SageMaker Debugger fully manages the built\-in rules and analyzes your training job in parallel\. For more information about billing, see the **Amazon SageMaker Studio is available at no additional charge** section of the [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/) page\.

In the following topics, learn how to use the Debugger built\-in rules\.

**Topics**
+ [Use Debugger Built\-in Rules with the Default Parameter Settings](#debugger-built-in-rules-configuration)
+ [Use Debugger Built\-in Rules with Custom Parameter Values](#debugger-built-in-rules-configuration-param-change)
+ [Example Notebooks and Code Samples to Configure Debugger Rules](#debugger-built-in-rules-example)

## Use Debugger Built\-in Rules with the Default Parameter Settings<a name="debugger-built-in-rules-configuration"></a>

To specify Debugger built\-in rules in an estimator, you need to configure a `rules` list object\. The following example code shows the basic structure of listing the Debugger built\-in rules:

```
from sagemaker.debugger import Rule, ProfilerRule, rule_configs

rules=[
    ProfilerRule.sagemaker(rule_configs.BuiltInProfilerRuleName_1()),
    ProfilerRule.sagemaker(rule_configs.BuiltInProfilerRuleName_2()),
    ...
    ProfilerRule.sagemaker(rule_configs.BuiltInProfilerRuleName_n()),
    Rule.sagemaker(rule_configs.built_in_rule_name_1()),
    Rule.sagemaker(rule_configs.built_in_rule_name_2()),
    ...
    Rule.sagemaker(rule_configs.built_in_rule_name_n())
]
```

For more information about default parameter values and descriptions of the built\-in rule, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

For example, to inspect the overall training performance and progress of your model, construct a SageMaker estimator with the following built\-in rule configuration\. 

```
from sagemaker.debugger import Rule, rule_configs

rules=[
    ProfilerRule.sagemaker(rule_configs.ProfilerReport()),
    Rule.sagemaker(rule_configs.loss_not_decreasing()),
    Rule.sagemaker(rule_configs.overfit()),
    Rule.sagemaker(rule_configs.overtraining()),
    Rule.sagemaker(rule_configs.stalled_training_rule())
]
```

When you start the training job, Debugger collects system resource utilization data every 500 milliseconds and the loss and accuracy values every 500 steps by default\. Debugger analyzes the resource utilization to identify if your model is having bottleneck problems\. The `loss_not_decreasing`, `overfit`, `overtraining`, and `stalled_training_rule` monitors if your model is optimizing the loss function without those training issues\. If the rules detect training anomalies, the rule evaluation status changes to `IssueFound`\. You can set up automated actions, such as notifying training issues and stopping training jobs using Amazon CloudWatch Events and AWS Lambda\. For more information, see [Action on Amazon SageMaker Debugger Rules](debugger-action-on-rules.md)\.



## Use Debugger Built\-in Rules with Custom Parameter Values<a name="debugger-built-in-rules-configuration-param-change"></a>

If you want to adjust the built\-in rule parameter values and customize tensor collection regex, configure the `base_config` and `rule_parameters` parameters for the `ProfilerRule.sagemaker` and `Rule.sagemaker` classmethods\. In case of the `Rule.sagemaker` class methods, you can also customize tensor collections through the `collections_to_save` parameter\. The instruction of how to use the `CollectionConfig` class is provided at [ Configure Debugger Tensor Collections Using the Collectivisation API Operation](debugger-configure-hook.md#debugger-configure-tensor-collections)\. 

Use the following configuration template for built\-in rules to customize parameter values\. By changing the rule parameters as you want, you can adjust the sensitivity of the rules to be triggered\. 
+ The `base_config` argument is where you call the built\-in rule methods\.
+ The `rule_parameters` argument is to adjust the default key values of the built\-in rules listed in [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.
+ The `collections_to_save` argument takes in a tensor configuration through the `CollectionConfig` API, which requires `name` and `parameters` arguments\. 
  + To find available tensor collections for `name`, see [ Debugger Built\-in Tensor Collections ](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md#built-in-collections)\. 
  + For a full list of adjustable `parameters`, see [ Debugger CollectionConfig API](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md#configuring-collection-using-sagemaker-python-sdk)\.

For more information about the Debugger rule class, methods, and parameters, see [SageMaker Debugger Rule class](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html) in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\.

```
from sagemaker.debugger import Rule, ProfilerRule, rule_configs, CollectionConfig

rules=[
    ProfilerRule.sagemaker(
        base_config=rule_configs.BuiltInProfilerRuleName(),
        rule_parameters={
                "key": "value"
        }
    )
    Rule.sagemaker(
        base_config=rule_configs.built_in_rule_name(),
        rule_parameters={
                "key": "value"
        }
        collections_to_save=[ 
            CollectionConfig(
                name="tensor_collection_name", 
                parameters={
                    "key": "value"
                } 
            )
        ]
    )
]
```

The parameter descriptions and value customization examples are provided for each rule at [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

## Example Notebooks and Code Samples to Configure Debugger Rules<a name="debugger-built-in-rules-example"></a>

In the following sections, notebooks and code samples of how to use Debugger rules to monitor SageMaker training jobs are provided\.

**Topics**
+ [Debugger Built\-in Rules Example Notebooks](#debugger-built-in-rules-notebook-example)
+ [Debugger Built\-in Rules Example Code](#debugger-deploy-built-in-rules)
+ [Use Debugger Built\-in Rules with Parameter Modifications](#debugger-deploy-modified-built-in-rules)

### Debugger Built\-in Rules Example Notebooks<a name="debugger-built-in-rules-notebook-example"></a>

The following example notebooks show how to use Debugger built\-in rules when running training jobs with Amazon SageMaker: 
+ [Using a SageMaker Debugger built\-in rule with TensorFlow](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_builtin_rule)
+ [Using a SageMaker Debugger built\-in rule with Managed Spot Training and MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mxnet_spot_training)
+ [Using a SageMaker Debugger built\-in rule with XGBoost](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_builtin_rules)
+ [Using a SageMaker Debugger built\-in rule with parameter modifications for a real\-time training job analysis with XGBoost](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_realtime_analysis)

While running the example notebooks in SageMaker Studio, you can find the training job trial created on the **Studio Experiment List** tab\. For example, as shown in the following screenshot, you can find and open a **Describe Trial Component** window of your current training job\. On the Debugger tab, you can check if the Debugger rules, `vanishing_gradient()` and `loss_not_decreasing()`, are monitoring the training session in parallel\. For a full instruction of how to find your training job trial components in the Studio UI, see [SageMaker Studio \- View Experiments, Trials, and Trial Components](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks.html#studio-tasks-experiments)\.

![\[An image of running a training job with Debugger built-in rules activated in SageMaker Studio\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger/debugger-built-in-rule-studio.png)

There are two ways of using the Debugger built\-in rules in the SageMaker environment: deploy the built\-in rules as it is prepared or adjust their parameters as you want\. The following topics show you how to use the built\-in rules with example codes\.

### Debugger Built\-in Rules Example Code<a name="debugger-deploy-built-in-rules"></a>

The following code sample shows how to set the Debugger built\-in rules using the `Rule.sagemaker` method\. To specify built\-in rules that you want to run, use the `rules_configs` API operation to call the built\-in rules\. To find a full list of Debugger built\-in rules and default parameter values, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

```
import sagemaker
from sagemaker.tensorflow import TensorFlow
from sagemaker.debugger import Rule, CollectionConfig, rule_configs

# call built-in rules that you want to use.
built_in_rules=[ 
            Rule.sagemaker(rule_configs.vanishing_gradient())
            Rule.sagemaker(rule_configs.loss_not_decreasing())
]

# construct a SageMaker estimator with the Debugger built-in rules
sagemaker_estimator=TensorFlow(
    entry_point='directory/to/your_training_script.py',
    role=sm.get_execution_role(),
    base_job_name='debugger-built-in-rules-demo',
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="2.1.0",
    py_version="py3",

    # debugger-specific arguments below
    rules=built_in_rules
)
sagemaker_estimator.fit()
```

**Note**  
The Debugger built\-in rules run in parallel with your training job\. The maximum number of built\-in rule containers for a training job is 20\. 

For more information about the Debugger rule class, methods, and parameters, see the [SageMaker Debugger Rule class](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html) in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. 

To find an example of how to adjust the Debugger rule parameters, see the following [Use Debugger Built\-in Rules with Parameter Modifications](#debugger-deploy-modified-built-in-rules) section\.

### Use Debugger Built\-in Rules with Parameter Modifications<a name="debugger-deploy-modified-built-in-rules"></a>

The following code example shows the structure of built\-in rules to adjust parameters\. In this example, the `stalled_training_rule` collects the `losses` tensor collection from a training job at every 50 steps and an evaluation stage at every 10 steps\. If the training process starts stalling and not collecting tensor outputs for 120 seconds, the `stalled_training_rule` stops the training job\. 

```
import sagemaker
from sagemaker.tensorflow import TensorFlow
from sagemaker.debugger import Rule, CollectionConfig, rule_configs

# call the built-in rules and modify the CollectionConfig parameters

base_job_name_prefix= 'smdebug-stalled-demo-' + str(int(time.time()))

built_in_rules_modified=[
    Rule.sagemaker(
        base_config=rule_configs.stalled_training_rule(),
        rule_parameters={
                'threshold': '120',
                'training_job_name_prefix': base_job_name_prefix,
                'stop_training_on_fire' : 'True'
        }
        collections_to_save=[ 
            CollectionConfig(
                name="losses", 
                parameters={
                      "train.save_interval": "50"
                      "eval.save_interval": "10"
                } 
            )
        ]
    )
]

# construct a SageMaker estimator with the modified Debugger built-in rule
sagemaker_estimator=TensorFlow(
    entry_point='directory/to/your_training_script.py',
    role=sm.get_execution_role(),
    base_job_name=base_job_name_prefix,
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="2.1.0",
    py_version="py3",

    # debugger-specific arguments below
    rules=built_in_rules_modified
)
sagemaker_estimator.fit()
```

For an advanced configuration of the Debugger built\-in rules using the `CreateTrainingJob` API, see [ Configure Debugger Using Amazon SageMaker APIConfigure Debugger Using SageMaker API   The preceding topics focus on using Debugger through Amazon SageMaker Python SDK, which is a wrapper around AWS SDK for Python \(Boto3\) and SageMaker API operations\. This offers a high\-level experience of accessing the Amazon SageMaker API operations\. In case you need to manually configure the SageMaker API operations using AWS Boto3 or AWS Command Line Interface \(CLI\) for other SDKs, such as Java, Go, and C\+\+, this guide covers how to configure [CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html), [UpdateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateTrainingJob.html), Debugger configuration APIs, and their parameters to use the Debugger built\-in and custom rules\.    JSON \(AWS CLI\) Amazon SageMaker Debugger built\-in rules can be configured for a training job using the [DebugHookConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html), [DebugRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html), [ProfilerConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerConfig.html), and [ProfilerRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerRuleConfiguration.html) objects through the SageMaker [CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API operation\. You need to specify the right image URI in the `RuleEvaluatorImage` parameter, and the following examples walk you through how to set up the JSON strings to request [CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)\. The following code shows a complete JSON template to run a training job with required settings and Debugger configurations\. Save the template as a JSON file in your working directory and run the training job using AWS CLI\. For example, save the following code as `debugger-training-job-cli.json`\. Ensure that you use the correct Docker container images\. To find AWS Deep Learning Container images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. 

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
``` After saving the JSON file, run the following command in your terminal\. \(Use `!` at the beginning of the line if you use a Jupyter notebook\.\) 

```
aws sagemaker create-training-job --cli-input-json file://debugger-training-job-cli.json
```  To configure a Debugger rule for debugging model parameters  The following code samples show how to configure a built\-in `VanishingGradient` rule using this SageMaker API\.  **To enable Debugger to collect output tensors** Specify the Debugger hook configuration as follows: 

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
``` This will make the training job save the tensor collection, `gradients`, every `save_interval` of 500 steps\. To find available `CollectionName` values, see [Debugger Built\-in Collections](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md#built-in-collections) in the *SMDebug client library documentation*\. To find available `CollectionParameters` parameter keys and values, see the [https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.CollectionConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.CollectionConfig) class in the *SageMaker Python SDK documentation*\. **To enable Debugger rules for debugging the output tensors** The following `DebugRuleConfigurations` API example shows how to run the built\-in `VanishingGradient` rule on the saved `gradients` collection\. 

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
``` With a configuration like the one in this sample, Debugger starts a rule evaluation job for your training job using the `VanishingGradient` rule on the collection of `gradients` tensor\. To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. To find the key\-value pairs for `RuleParameters`, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.   To configure a Debugger built\-in rule for profiling system and framework metrics  The following example code shows how to specify the ProfilerConfig API operation to enable collecting system and framework metrics\. **To enable Debugger profiling to collect system and framework metrics**   Target Step  

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
```    Target Time Duration  

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
```    **To enable Debugger rules for profiling the metrics** The following example code shows how to configure the `ProfilerReport` rule\. 

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
``` To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. To find the key\-value pairs for `RuleParameters`, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.   Update Debugger Profiling Configuration Using the `UpdateTrainingJob` API Operation  Debugger profiling configuration can be updated while your training job is running by using the [UpdateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_UpdateTrainingJob.html) API operation\. Configure new [ProfilerConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerConfig.html) and [ProfilerRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerRuleConfiguration.html) objects, and specify the training job name to the `TrainingJobName` parameter\. 

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
```   Add Debugger Custom Rule Configuration to the CreateTrainingJob API Operation  A custom rule can be configured for a training job using the [ DebugHookConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html) and [ DebugRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html) objects in the [ CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API operation\. The following code sample shows how to configure a custom `ImproperActivation` rule written with the *smdebug* library using this SageMaker API operation\. This example assumes that you’ve written the custom rule in *custom\_rules\.py* file and uploaded it to an Amazon S3 bucket\. The example provides pre\-built Docker images that you can use to run your custom rules\. These are listed at [Amazon SageMaker Debugger Registry URLs for Custom Rule Evaluators](debugger-docker-images-rules.md#debuger-custom-rule-registry-ids)\. You specify the URL registry address for the pre\-built Docker image in the `RuleEvaluatorImage` parameter\. 

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
``` To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. To find the key\-value pairs for `RuleParameters`, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.    AWS Boto3 Amazon SageMaker Debugger built\-in rules can be configured for a training job using the [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job) function of the AWS Boto3 SageMaker client\. You need to specify the right image URI in the `RuleEvaluatorImage` parameter, and the following examples walk you through how to set up the request body for the [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job) function\. The following code shows a complete example of how to configure Debugger for the `create_training_job()` request body and start a training job in `us-west-2`, assuming that a training script `entry_point/train.py` is prepared using TensorFlow\. To find an end\-to\-end example notebook, see [Profiling TensorFlow Multi GPU Multi Node Training Job with Amazon SageMaker Debugger \(Boto3\)](https://github.com/aws/amazon-sagemaker-examples/blob/master/sagemaker-debugger/tensorflow_profiling/tf-resnet-profiling-multi-gpu-multi-node-boto3.ipynb)\. Ensure that you use the correct Docker container images\. To find available AWS Deep Learning Container images, see [Available Deep Learning Containers Images](https://github.com/aws/deep-learning-containers/blob/master/available_images.md)\. To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. 

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
```  To configure a Debugger rule for debugging model parameters  The following code samples show how to configure a built\-in `VanishingGradient` rule using this SageMaker API\.  **To enable Debugger to collect output tensors** Specify the Debugger hook configuration as follows: 

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
``` This will make the training job save a tensor collection, `gradients`, every `save_interval` of 500 steps\. To find available `CollectionName` values, see [Debugger Built\-in Collections](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md#built-in-collections) in the *SMDebug client library documentation*\. To find available `CollectionParameters` parameter keys and values, see the [https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.CollectionConfig](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.CollectionConfig) class in the *SageMaker Python SDK documentation*\. **To enable Debugger rules for debugging the output tensors** The following `DebugRuleConfigurations` API example shows how to run the built\-in `VanishingGradient` rule on the saved `gradients` collection\. 

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
``` With a configuration like the one in this sample, Debugger starts a rule evaluation job for your training job using the `VanishingGradient` rule on the collection of `gradients` tensor\. To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. To find the key\-value pairs for `RuleParameters`, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.   To configure a Debugger built\-in rule for profiling system and framework metrics  The following example code shows how to specify the ProfilerConfig API operation to enable collecting system and framework metrics\. **To enable Debugger profiling to collect system and framework metrics**   Target Step 

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
```    Target Time Duration 

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
```    **To enable Debugger rules for profiling the metrics** The following example code shows how to configure the `ProfilerReport` rule\. 

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
``` To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. To find the key\-value pairs for `RuleParameters`, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.   Update Debugger Profiling Configuration Using the `UpdateTrainingJob` API Operation  Debugger profiling configuration can be updated while your training job is running by using the [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.update_training_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.update_training_job) function of the AWS Boto3 SageMaker client\. Configure new [ProfilerConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerConfig.html) and [ProfilerRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ProfilerRuleConfiguration.html) objects, and specify the training job name to the `TrainingJobName` parameter\. 

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
```   Add Debugger Custom Rule Configuration to the CreateTrainingJob API Operation  A custom rule can be configured for a training job using the [ DebugHookConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html) and [ DebugRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html) objects using the AWS Boto3 SageMaker client's [https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sagemaker.html#SageMaker.Client.create_training_job) function\. The following code sample shows how to configure a custom `ImproperActivation` rule written with the *smdebug* library using this SageMaker API operation\. This example assumes that you’ve written the custom rule in *custom\_rules\.py* file and uploaded it to an Amazon S3 bucket\. The example provides pre\-built Docker images that you can use to run your custom rules\. These are listed at [Amazon SageMaker Debugger Registry URLs for Custom Rule Evaluators](debugger-docker-images-rules.md#debuger-custom-rule-registry-ids)\. You specify the URL registry address for the pre\-built Docker image in the `RuleEvaluatorImage` parameter\. 

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
``` To find a complete list of available Docker images for using the Debugger rules, see [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md)\. To find the key\-value pairs for `RuleParameters`, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.   ](debugger-createtrainingjob-api.md)\.