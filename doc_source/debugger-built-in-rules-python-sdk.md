# Use [*smdebug* Python library](https://github.com/awslabs/sagemaker-debugger/) with the Amazon SageMaker Python SDK to Create a Built\-in Rule<a name="debugger-built-in-rules-python-sdk"></a>

For a sample notebook that shows you how to use a built\-in rule when training job with a TensorFlow model in SM; see [Amazon SageMaker Debugger \- Using built\-in rule](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_builtin_rule)\. 

The following code sample shows how to run an Amazon SageMaker built\-in `Rule` using the `Rule.sagemaker` method from the Amazon SageMaker Python SDK\. The first argument to this method is the base configuration that is associated with the `Rule`\. We configure the built\-in rules with the `smdebug_ruleconfigs` that we populate for all of the built\-in rules\. For details, see [Amazon SageMaker Debugger RulesConfig](https://github.com/awslabs/sagemaker-debugger-rulesconfig/blob/master/README.md)\. We provide default values for the parameters that are not required, but you have the option to modify these default values when using the built\-in rules\. You do not need to specify a pre\-built Docker image when using the Amazon SageMaker Python SDK as the `Estimators` handle this task\. 

```
from sagemaker.debugger import Rule, CollectionConfig, rule_configs

exploding_tensor_rule = Rule.sagemaker(
    base_config=rule_configs.exploding_tensor(),
    rule_parameters={"collection_names": "weights,losses"},
    collections_to_save=[
        CollectionConfig("weights"),
        CollectionConfig("losses")
    ]
)

vanishing_gradient_rule = Rule.sagemaker(
    base_config=rule_configs.vanishing_gradient()
)

import sagemaker as sm
sagemaker_estimator = sm.tensorflow.TensorFlow(
    entry_point='src/mnist.py',
    role=sm.get_execution_role(),
    base_job_name='smdebug-demo-job',
    train_instance_count=1,
    train_instance_type="ml.m4.xlarge",
    framework_version="1.15",
    py_version="py3",
    # smdebug-specific arguments below
    rules=[exploding_tensor_rule, vanishing_gradient_rule],
)
sagemaker_estimator.fit()
```

You can use these rules or write your own rules using the Amazon SageMaker Debugger APIs\. You can also analyze raw tensor data without using rules in, for example, an Amazon SageMaker notebook, using Debugger's full set of APIs\. 