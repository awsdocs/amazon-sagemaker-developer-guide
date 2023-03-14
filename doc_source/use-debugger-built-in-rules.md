# Configure Debugger Built\-in Rules<a name="use-debugger-built-in-rules"></a>

Amazon SageMaker Debugger's built\-in rules analyze tensors emitted during the training of a model\. SageMaker Debugger offers the `Rule` API operation that monitors training job progress and errors for the success of training your model\. For example, the rules can detect whether gradients are getting too large or too small, whether a model is overfitting or overtraining, and whether a training job does not decrease loss function and improve\. To see a full list of available built\-in rules, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

In the following topics, you'll learn how to use the SageMaker Debugger built\-in rules\.

**Topics**
+ [Use Debugger Built\-in Rules with the Default Parameter Settings](#debugger-built-in-rules-configuration)
+ [Use Debugger Built\-in Rules with Custom Parameter Values](#debugger-built-in-rules-configuration-param-change)
+ [Example Notebooks and Code Samples to Configure Debugger Rules](#debugger-built-in-rules-example)

## Use Debugger Built\-in Rules with the Default Parameter Settings<a name="debugger-built-in-rules-configuration"></a>

To specify Debugger built\-in rules in an estimator, you need to configure a list object\. The following example code shows the basic structure of listing the Debugger built\-in rules:

```
from sagemaker.debugger import Rule, rule_configs

rules=[
    Rule.sagemaker(rule_configs.built_in_rule_name_1()),
    Rule.sagemaker(rule_configs.built_in_rule_name_2()),
    ...
    Rule.sagemaker(rule_configs.built_in_rule_name_n()),
    ... # You can also append more profiler rules in the ProfilerRule.sagemaker(rule_configs.*()) format.
]
```

For more information about default parameter values and descriptions of the built\-in rule, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

To find the SageMaker Debugger API reference, see [https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.sagemaker.debugger.rule_configs](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.sagemaker.debugger.rule_configs) and [https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.Rule](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.Rule)\.

For example, to inspect the overall training performance and progress of your model, construct a SageMaker estimator with the following built\-in rule configuration\. 

```
from sagemaker.debugger import Rule, rule_configs

rules=[
    Rule.sagemaker(rule_configs.loss_not_decreasing()),
    Rule.sagemaker(rule_configs.overfit()),
    Rule.sagemaker(rule_configs.overtraining()),
    Rule.sagemaker(rule_configs.stalled_training_rule())
]
```

When you start the training job, Debugger collects system resource utilization data every 500 milliseconds and the loss and accuracy values every 500 steps by default\. Debugger analyzes the resource utilization to identify if your model is having bottleneck problems\. The `loss_not_decreasing`, `overfit`, `overtraining`, and `stalled_training_rule` monitors if your model is optimizing the loss function without those training issues\. If the rules detect training anomalies, the rule evaluation status changes to `IssueFound`\. You can set up automated actions, such as notifying training issues and stopping training jobs using Amazon CloudWatch Events and AWS Lambda\. For more information, see [Action on Amazon SageMaker Debugger Rules](debugger-action-on-rules.md)\.



## Use Debugger Built\-in Rules with Custom Parameter Values<a name="debugger-built-in-rules-configuration-param-change"></a>

If you want to adjust the built\-in rule parameter values and customize tensor collection regex, configure the `base_config` and `rule_parameters` parameters for the `ProfilerRule.sagemaker` and `Rule.sagemaker` classmethods\. In case of the `Rule.sagemaker` class methods, you can also customize tensor collections through the `collections_to_save` parameter\. The instruction of how to use the `CollectionConfig` class is provided at [Configure Tensor Collections Using the `CollectionConfig` API](debugger-configure-hook.md#debugger-configure-tensor-collections)\. 

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
    Rule.sagemaker(
        base_config=rule_configs.built_in_rule_name(),
        rule_parameters={
                "key": "value"
        },
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
    instance_count=1,
    instance_type="ml.p3.2xlarge",
    framework_version="2.9.0",
    py_version="py39",

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
    instance_count=1,
    instance_type="ml.p3.2xlarge",
    framework_version="2.9.0",
    py_version="py39",

    # debugger-specific arguments below
    rules=built_in_rules_modified
)
sagemaker_estimator.fit()
```

For an advanced configuration of the Debugger built\-in rules using the `CreateTrainingJob` API, see [Configure Debugger Using Amazon SageMaker API](debugger-createtrainingjob-api.md)\.