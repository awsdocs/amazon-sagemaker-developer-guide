# Use Debugger Built\-in Rules for Training Job Analysis<a name="use-debugger-built-in-rules"></a>

Amazon SageMaker Debugger rules analyze tensors emitted during the training of a model\. Debugger offers the `Rule` API operation that monitors training job progress and errors for the success of training your model\. For example, the rules can detect whether gradients are getting too large or too small, whether a model is overfitting or overtraining, and whether a training job does not decrease loss function and improve\. Debugger `Rules` provides more than a dozen of convenient built\-in rules that you can directly apply to your training jobs\.

**Note**  
The Debugger built\-in rules are prepared in Amazon SageMaker containers\. A full list of available pre\-built docker images for the built\-in rules is provided at [ Amazon SageMaker Debugger Registry URLs for Built\-in Rule Evaluators](https://docs.aws.amazon.com/sagemaker/latest/dg/debuger-built-in-registry-ids.html)\. If you use the pre\-built AWS containers, the step where you construct SageMaker estimators automatically calls the rule images and you do not need to manually specify it\. You can also use all of the built\-in rules without any additional cost when using the AWS managed containers\.

**Topics**
+ [Debugger Built\-in Rules Example Notebooks](#debugger-built-in-rules-notebook-example)
+ [Use Debugger Built\-in Rules](#debugger-deploy-built-in-rules)
+ [Use Debugger Built\-in Rules with Parameter Modifications](#debugger-deploy-modified-built-in-rules)

## Debugger Built\-in Rules Example Notebooks<a name="debugger-built-in-rules-notebook-example"></a>

The following example notebooks show how to use Debugger built\-in rules when running training jobs with Amazon SageMaker: 
+ [Using a SageMaker Debugger built\-in rule with TensorFlow](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_builtin_rule)
+ [Using a SageMaker Debugger built\-in rule with Managed Spot Training and MXNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/mxnet_spot_training)
+ [Using a SageMaker Debugger built\-in rule with XGBoost](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_builtin_rules)
+ [Using a SageMaker Debugger built\-in rule with parameter modifications for a real\-time training job analysis with XGBoost](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/xgboost_realtime_analysis)

 While running the example notebooks in SageMaker Studio, you can find the training job trial created on the **Studio Experiment List** tab\. For example, as shown in the following screenshot, you can find and open a **Describe Trial Component** window of your current training job\. On the Debugger tab, you can check if the Debugger rules, `vanishing_gradient()` and `loss_not_decreasing()`, are monitoring the training session in parallel\. For a full instruction of how to find your training job trial components in the Studio UI, see [SageMaker Studio \- View Experiments, Trials, and Trial Components](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks.html#studio-tasks-experiments)\.

![\[An image of running a training job with Debugger built-in rules activated in SageMaker Studio\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/debugger-built-in-rule-studio.png)

There are two ways of using the Debugger built\-in rules in the SageMaker environment: deploy the built\-in rules as it is prepared or adjust their parameters as you want\. The following topics show you how to use the built\-in rules with example codes\.

## Use Debugger Built\-in Rules<a name="debugger-deploy-built-in-rules"></a>

The following code sample shows how to deploy the Debugger built\-in rules using the `Rule.sagemaker` method from the [Amazon SageMaker Debugger Python SDK](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html)\. To specify built\-in rules that you want to run, simply use the `rules_configs` API operation to call the rule methods\. You do not need to specify pre\-built Docker images for the rules when using [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) that handles the task\. 

```
import sagemaker
from sagemaker.tensorflow import TensorFlow
from sagemaker.debugger import Rule, CollectionConfig, rule_configs

# call built-in rules that you want to use.
built_in_rules = [ 
            Rule.sagemaker(rule_configs.vanishing_gradient())
            Rule.sagemaker(rule_configs.loss_not_decreasing())
]

# construct a SageMaker estimator with the Debugger built-in rules
sagemaker_estimator = TensorFlow(
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
The number of rules you can run in parallel run on `ml.t3.medium` instances\. The maximum number of built\-in rule containers for a training job is 20\. You cannot specify other instance types for the Debugger built\-in rules\.

To find a full list of Debugger built\-in rules in SageMaker Python SDK, see [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.

## Use Debugger Built\-in Rules with Parameter Modifications<a name="debugger-deploy-modified-built-in-rules"></a>

If you want to modify the built\-in rules to adjust parameter values, collect specific tensors, change the tensor save intervals, set start and end steps, and monitor different stages of training jobs, you need to specify `base_config`, `rule_parameters`, and `collections_to_save` arguments for the `Rule.sagemaker` method\.
+ The `base_config` argument is where you call the built\-in rule methods\.
+ The `rule_parameters` argument is to adjust the default key values of the built\-in rules listed in [List of Debugger Built\-in Rules](debugger-built-in-rules.md)\.
+ The `collections_to_save` argument takes in a tensor configuration through the `CollectionConfig` API, which requires `name` and `parameters` arguments\. 
  + To find available tensor collections for `name`, see [ Debugger Built\-in Tensor Collections ](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md#built-in-collections)\. 
  + For a full list of adjustable `parameters`, see [ Debugger CollectionConfig API](https://github.com/awslabs/sagemaker-debugger/blob/master/docs/api.md#configuring-collection-using-sagemaker-python-sdk)\.

The following code example shows the structure of built\-in rules to adjust parameters\. In this example, the `stalled_training_rule` collects the `losses` tensor collection from a training stage at every 50 step and an evaluation stage at every 10 step\. If the training process starts stalling, the `stalled_training_rule` stops training job if there is no tensor output collected for 120 seconds\. 

```
import sagemaker
from sagemaker.tensorflow import TensorFlow
from sagemaker.debugger import Rule, CollectionConfig, rule_configs

# call the built-in rules and modify the CollectionConfig parameters
built_in_rules_modified = [
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
                      "train.save_interval": "50"} )])]

# construct a SageMaker estimator with the modified Debugger built-in rule
sagemaker_estimator = TensorFlow(
    entry_point='directory/to/your_training_script.py',
    role=sm.get_execution_role(),
    base_job_name='debugger-built-in-rules-demo',
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="2.1.0",
    py_version="py3",

    # debugger-specific arguments below
    rules=built_in_rules_modified
)
sagemaker_estimator.fit()
```

For an advanced configuration of the Debugger built\-in rules using the `CreateTrainingJob` API, see [Add Debugger Built\-in Rule Configuration to the `CreateTrainingJob` API Operation](debugger-createtrainingjob-api.md#debugger-built-in-rules-api)\.