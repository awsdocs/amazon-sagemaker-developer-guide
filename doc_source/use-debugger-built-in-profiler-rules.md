# Configure Built\-in Profiler Rules Managed by Amazon SageMaker Debugger<a name="use-debugger-built-in-profiler-rules"></a>

Amazon SageMaker Debugger's built\-in profiler rules analyze system metrics and framework operations collected during the training of a model\. Debugger offers the `ProfilerRule` API operation that helps configure the rules to monitor training job progress and detect anomalies\. For example, the rules can detect whether gradients are getting too large or too small, whether a model is overfitting or overtraining, and whether a training job does not decrease loss function and improve\. To see a full list of available built\-in rules, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

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
+ [Use Debugger Built\-in Profiler Rules with the Default Parameter Settings](#debugger-built-in-profiler-rules-configuration)
+ [Use Debugger Built\-in Profiler Rules with Custom Parameter Values](#debugger-built-in-profiler-rules-configuration-param-change)

## Use Debugger Built\-in Profiler Rules with the Default Parameter Settings<a name="debugger-built-in-profiler-rules-configuration"></a>

To specify Debugger built\-in rules in an estimator, you need to configure a `rules` list object\. The following example code shows the basic structure of listing the Debugger built\-in rules:

```
from sagemaker.debugger import Rule, ProfilerRule, rule_configs

rules=[
    ProfilerRule.sagemaker(rule_configs.BuiltInProfilerRuleName_1()),
    ProfilerRule.sagemaker(rule_configs.BuiltInProfilerRuleName_2()),
    ...
    ProfilerRule.sagemaker(rule_configs.BuiltInProfilerRuleName_n()),
    ... # You can also append more debugging rules in the Rule.sagemaker(rule_configs.*()) format.
]
```

For more information about default parameter values and descriptions of the built\-in rule, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

For example, to inspect the overall training performance and progress of your model, construct a SageMaker estimator with the following built\-in rule configuration\. 

```
from sagemaker.debugger import Rule, rule_configs

rules=[
    ProfilerRule.sagemaker(rule_configs.ProfilerReport())
]
```

When you start the training job, Debugger collects system resource utilization data every 500 milliseconds and the loss and accuracy values every 500 steps by default\. Debugger analyzes the resource utilization to identify if your model is having bottleneck problems\. The `loss_not_decreasing`, `overfit`, `overtraining`, and `stalled_training_rule` monitors if your model is optimizing the loss function without those training issues\. If the rules detect training anomalies, the rule evaluation status changes to `IssueFound`\. You can set up automated actions, such as notifying training issues and stopping training jobs using Amazon CloudWatch Events and AWS Lambda\. For more information, see [Action on Amazon SageMaker Debugger Rules](debugger-action-on-rules.md)\.



## Use Debugger Built\-in Profiler Rules with Custom Parameter Values<a name="debugger-built-in-profiler-rules-configuration-param-change"></a>

If you want to adjust the built\-in rule parameter values and customize tensor collection regex, configure the `base_config` and `rule_parameters` parameters for the `ProfilerRule.sagemaker` and `Rule.sagemaker` classmethods\. In case of the `Rule.sagemaker` class methods, you can also customize tensor collections through the `collections_to_save` parameter\. The instruction of how to use the `CollectionConfig` class is provided at [Configure Tensor Collections Using the `CollectionConfig` API](debugger-configure-hook.md#debugger-configure-tensor-collections)\. 

Use the following configuration template for built\-in rules to customize parameter values\. By changing the rule parameters as you want, you can adjust the sensitivity of the rules to be triggered\. 
+ The `base_config` argument is where you call the built\-in rule methods\.
+ The `rule_parameters` argument is to adjust the default key values of the built\-in rules listed in [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

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
]
```

The parameter descriptions and value customization examples are provided for each rule at [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

For a low\-level JSON configuration of the Debugger built\-in rules using the `CreateTrainingJob` API, see [Configure Debugger Using Amazon SageMaker API](debugger-createtrainingjob-api.md)\.