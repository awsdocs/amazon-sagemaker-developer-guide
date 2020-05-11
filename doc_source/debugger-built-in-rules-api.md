# Use the `CreateTrainingJob` API to Create a Built\-in Rule<a name="debugger-built-in-rules-api"></a>

A built\-in rule can be configured for a training job using the [ `DebugHookConfig`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugHookConfig.html) and [ `DebugRuleConfiguration`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_DebugRuleConfiguration.html) objects in the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) API\. These rules are run on one of our pre\-built Docker images which are listed in the [Use Amazon SageMaker Debugger Images for Built\-in or Custom Rules](debugger-docker-images-rules.md) topic\. You specify the URL registry address for the pre\-built Docker image in the `RuleEvaluatorImage` parameter\.

The following code sample shows how to configure a built\-in `VanishingGradient` rule using this Amazon SageMaker API\.

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

With a configuration like the one in this sample, Amazon SageMaker Debugger starts a rule evaluation job for your training job using the Amazon SageMaker VanishingGradient rule\.