# Use the SageMaker CreateTrainingJob and Debugger Configuration API Operations to Create and Debug Your Training Job<a name="debugger-createtrainingjob-api"></a>

 The preceding topics focused on using Debugger through Amazon SageMaker Python SDK, which is a wrapper around AWS boto3 API operations for SageMaker\. This offers a high\-level experience of accessing the Amazon SageMaker API operations\. In case you need to use the SageMaker API operations directly with other SDKs, such as Java, Go, C\+\+, and many others, the following topics cover how to configure the Debugger APIs, `DebugHookConfig` and `DebugRuleConfiguration`, and their parameters to use the Debugger built\-in and custom rules\. 

## Add Debugger Built\-in Rule Configuration to the `CreateTrainingJob` API Operation<a name="debugger-built-in-rules-api"></a>

Amazon SageMaker Debugger built\-in rules can be configured for a training job using the [ DebugHookConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html) and [ DebugRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html) objects in the [ CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API operation\. The built\-in rules run on independent pre\-built Docker containers listed in the [Use Debugger Docker Images for Built\-in or Custom Rules](debugger-docker-images-rules.md) topic\. You specify the URI address for the pre\-built Docker images in the `RuleEvaluatorImage` parameter\.

The following code sample shows how to configure a built\-in `VanishingGradient` rule using this SageMaker API\. Specify the Debugger hook configuration as follows:

```
DebugHookConfig: {
    "S3OutputPath": "s3://bucket/path-to-tensors",
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

This will make the training job save the tensor collection, `gradients`, every `save_interval` of 500 steps\. The following code example of the `DebugRuleConfigurations` API demonstrates how to run the built\-in `VanishingGradient` rule on the saved `gradients` collection\.

```
DebugRuleConfigurations: [
  {
    "RuleConfigurationName": "Amazon-VanishingGradient",
    "RuleEvaluatorImage": "503895931360.dkr.ecr.us-east-1.amazonaws.com/sagemaker-debugger-rules:latest",
	"RuleParameters": {
   	"rule_to_invoke": "VanishingGradient",
   	"threshold": "20.0"
    }
  }
]
```

With a configuration like the one in this sample, Amazon SageMaker Debugger starts a rule evaluation job for your training job using the SageMaker `VanishingGradient` rule on the collection of `gradients` tensor\.

## Add Debugger Custom Rule Configuration to the CreateTrainingJob API Operation<a name="debugger-custom-rules-api"></a>

A custom rule can be configured for a training job using the [ DebugHookConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html) and [ DebugRuleConfiguration](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html) objects in the [ CreateTrainingJob](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API operation\. The following code sample shows how to configure a custom `ImproperActivation` rule written with the *smdebug* library using this SageMaker API operation\. This example assumes that you’ve written the custom rule in *custom\_rules\.py* file and uploaded it to an Amazon S3 bucket\. The example provides pre\-built Docker images that you can use to run your custom rules\. These are listed at [Amazon SageMaker Debugger Registry URLs for Custom Rule Evaluators](debugger-docker-images-rules.md#debuger-custom-rule-registry-ids)\. You specify the URL registry address for the pre\-built Docker image in the `RuleEvaluatorImage` parameter\.

```
DebugHookConfig: {
     "S3OutputPath": "s3://bucket/",
     "CollectionConfigurations": [
         {
             "CollectionName": "CustomCollection",
             "CollectionParameters": {
                 "include_regex": "relu",
                 "save_interval": "500",
                 "end_step": "5000"
             }
         }
     ]
},
DebugRulesConfigurations: [
    {
        "RuleConfigurationName": "improper_activation_job",
        "RuleEvaluatorImage": "552407032007.dkr.ecr.ap-south-1.amazonaws.com/sagemaker-debugger-rule-evaluator:latest"
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