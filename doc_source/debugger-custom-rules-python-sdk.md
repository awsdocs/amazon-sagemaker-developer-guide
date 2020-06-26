# Use [*smdebug* Python library](https://github.com/awslabs/sagemaker-debugger/) with the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) to Create a Custom Rule<a name="debugger-custom-rules-python-sdk"></a>

For a sample notebook that shows you how to use a custom rule to monitor your training job with a tf\.keras ResNet example, see [Amazon SageMaker \- Debugging with custom rules](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_keras_custom_rule)\. 

The following code sample shows how to configure a custom `ImproperActivation` rule using the open source[ *smdebug* Python library](https://github.com/awslabs/sagemaker-debugger/) with the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. This example assumes that the custom rule you’ve written has path */rules/custom\_rules\.py*\. You do not need to specify a pre\-built Docker image when using the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) as the `Estimators` handle this task\. 

```
from sagemaker.debugger import Rule, CollectionConfig

custom_coll = CollectionConfig(
    name="relu_activations",
    parameters={
        "include_regex": "relu",
        "save_interval": "500",
        "end_step": "5000"
    })
improper_activation_rule = Rule.custom(
    name='improper_activation_job',
    image_uri='552407032007.dkr.ecr.ap-south-1.amazonaws.com/sagemaker-debugger-rule-evaluator:latest',
    instance_type='ml.c4.xlarge',
    volume_size_in_gb=400,
    source='rules/custom_rules.py',
    rule_to_invoke='ImproperActivation',
    rule_parameters={"collection_names": "relu_activations"},
    collections_to_save=[custom_coll]
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
    rules=[improper_activation_rule],
)
sagemaker_estimator.fit()
```