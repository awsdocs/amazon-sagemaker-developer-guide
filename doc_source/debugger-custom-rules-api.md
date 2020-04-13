# Use the `CreateTrainingJob` API to Create a Custom Rule<a name="debugger-custom-rules-api"></a>

A custom rule can be configured for a training job using the [ `DebugHookConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html) and [ `DebugRuleConfiguration`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html) objects in the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API\. The following code sample shows how to configure a custom `ImproperActivation` rule written with the *smdebug* library using this Amazon SageMaker API\. This example assumes that you’ve written the custom rule in *custom\_rules\.py* file and uploaded it to an S3 bucket\. We provide pre\-built Docker images that you can use to run your custom rules\. These are listed at [Amazon SageMaker Debugger Registry URLs for Custom Rule Evaluators](debuger-custom-rule-registry-ids.md)\. You specify the URL registry address for the pre\-built Docker image in the `RuleEvaluatorImage` parameter\.

```
DebugHookConfig: {
     "S3OutputPath": "s3://bucket/",
     "CollectionConfigurations": [
         {
             "CustomCollection": "",
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