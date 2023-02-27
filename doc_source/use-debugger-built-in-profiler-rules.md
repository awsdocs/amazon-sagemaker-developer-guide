# Configure Built\-in Profiling Rules Managed by Amazon SageMaker Debugger<a name="use-debugger-built-in-profiler-rules"></a>

The Amazon SageMaker Debugger built\-in profiling rules analyze system metrics and framework operations collected during the training of a model\. Debugger offers the `ProfilerRule` API operation that helps configure the rules to monitor training compute resources and operations and to detect anomalies\. For example, the profiling rules can help you detect whether there are computational problems such as CPU bottlenecks, excessive I/O wait time, imbalanced workload across GPU workers, and compute resource underutilization\. To see a full list of available built\-in profiling rules, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

**Note**  
The built\-in rules are provided through Amazon SageMaker processing containers and fully managed by SageMaker Debugger at no additional cost\. For more information about billing, see the [Amazon SageMaker Pricing](https://aws.amazon.com/sagemaker/pricing/) page\.

In the following topics, learn how to use the Debugger built\-in rules\.

**Topics**
+ [Use SageMaker Debugger Built\-in Profiler Rules with the Default Parameter Settings](#debugger-built-in-profiler-rules-configuration)
+ [Use Debugger Built\-in Profiler Rules with Custom Parameter Values](#debugger-built-in-profiler-rules-configuration-param-change)

## Use SageMaker Debugger Built\-in Profiler Rules with the Default Parameter Settings<a name="debugger-built-in-profiler-rules-configuration"></a>

To add SageMaker Debugger built\-in rules in your estimator, you need to configure a `rules` list object\. The following example code shows the basic structure of listing the SageMaker Debugger built\-in rules\.

```
from sagemaker.debugger import Rule, ProfilerRule, rule_configs

rules=[
    ProfilerRule.sagemaker(rule_configs.BuiltInProfilerRuleName_1()),
    ProfilerRule.sagemaker(rule_configs.BuiltInProfilerRuleName_2()),
    ...
    ProfilerRule.sagemaker(rule_configs.BuiltInProfilerRuleName_n()),
    ... # You can also append more debugging rules in the Rule.sagemaker(rule_configs.*()) format.
]

estimator=Estimator(
    ...
    rules=rules
)
```

For a complete list of available built\-in rules, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

To use the profiling rules and inspect the computational performance and progress of your training job, add the [https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html#profiler-report](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html#profiler-report) rule of SageMaker Debugger\. This rule activates all built\-in rules under the [Debugger ProfilerRule](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html#debugger-built-in-rules-ProfilerRule) `ProfilerRule` family\. Furthermore, this rule generates an aggregated profiling report\. For more information, see [Profiling Report Generated Using SageMaker Debugger](https://docs.aws.amazon.com/sagemaker/latest/dg/debugger-profiling-report.html)\. You can use the following code to add the profiling report rule to your training estimator\.

```
from sagemaker.debugger import Rule, rule_configs

rules=[
    ProfilerRule.sagemaker(rule_configs.ProfilerReport())
]
```

When you start the training job with the `ProfilerReport` rule, Debugger collects resource utilization data every 500 milliseconds\. Debugger analyzes the resource utilization to identify if your model is having bottleneck problems\. If the rules detect training anomalies, the rule evaluation status changes to `IssueFound`\. You can set up automated actions, such as notifying training issues and stopping training jobs using Amazon CloudWatch Events and AWS Lambda\. For more information, see [Action on Amazon SageMaker Debugger Rules](debugger-action-on-rules.md)\.

## Use Debugger Built\-in Profiler Rules with Custom Parameter Values<a name="debugger-built-in-profiler-rules-configuration-param-change"></a>

If you want to adjust the built\-in rule parameter values and customize tensor collection regex, configure the `base_config` and `rule_parameters` parameters for the `ProfilerRule.sagemaker` and `Rule.sagemaker` class methods\. In case of the `Rule.sagemaker` class methods, you can also customize tensor collections through the `collections_to_save` parameter\. For instruction on how to use the `CollectionConfig` class, see [Configure Tensor Collections Using the `CollectionConfig` API](debugger-configure-hook.md#debugger-configure-tensor-collections)\. 

Use the following configuration template for built\-in rules to customize parameter values\. By changing the rule parameters as you want, you can adjust the sensitivity of the rules to be initiated\. 
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