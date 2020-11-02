# Create Debugger Custom Rules for Training Job Analysis<a name="debugger-custom-rules"></a>

You can create custom rules to monitor your training job using the Debugger Rule APIs and the open source [smdebug Python library](https://github.com/awslabs/sagemaker-debugger/) that provide tools to build your own rule containers\. 

**Topics**
+ [Prerequisites for Creating Debugger Custom Rules](#debugger-custom-rules-prerequisite)
+ [Use the Debugger Client Library `smdebug` to Create a Custom Rule Python Script](#debugger-custom-rules-python-script)
+ [Use the Debugger APIs to Run Your Own Custom Rules](#debugger-custom-rules-python-sdk)

## Prerequisites for Creating Debugger Custom Rules<a name="debugger-custom-rules-prerequisite"></a>

To create Debugger custom rules, you need the following prerequisites\.
+ [SageMaker Debugger Rule\.custom API](https://sagemaker.readthedocs.io/en/stable/api/training/debugger.html#sagemaker.debugger.Rule.custom)
+ [The open source smdebug Python library](https://github.com/awslabs/sagemaker-debugger/)
+ Your own custom rule python script
+ [Amazon SageMaker Debugger Registry URLs for Custom Rule Evaluators](debugger-docker-images-rules.md#debuger-custom-rule-registry-ids)

## Use the Debugger Client Library `smdebug` to Create a Custom Rule Python Script<a name="debugger-custom-rules-python-script"></a>

The `smdebug` Rule API provides an interface to set up your own custom rules\. The following python script is a sample of how to construct a custom rule, `CustomGradientRule`\. This tutorial custom rule watches if the gradients are getting too large and set the default threshold as 10\. The custom rule takes a base trial created by a SageMaker estimator when it initiates training job\. 

```
from smdebug.rules.rule import Rule

class CustomGradientRule(Rule):
    def __init__(self, base_trial, threshold=10.0):
        super().__init__(base_trial)
        self.threshold = float(threshold)

    def invoke_at_step(self, step):
        for tname in self.base_trial.tensor_names(collection="gradients"):
            t = self.base_trial.tensor(tname)
            abs_mean = t.reduction_value(step, "mean", abs=True)
            if abs_mean > self.threshold:
                return True
        return False
```

You can add multiple custom rule classes as many as you want in the same python script and deploy them to any training job trials by constructing custom rule objects in the following section\.

## Use the Debugger APIs to Run Your Own Custom Rules<a name="debugger-custom-rules-python-sdk"></a>

The following code sample shows how to configure a custom rule with the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io)\. This example assumes that the custom rule script you created in the previous step is located at '*path/to/my\_custom\_rule\.py*'\.

```
from sagemaker.debugger import Rule, CollectionConfig

custom_rule = Rule.custom(
    name='MyCustomRule',
    image_uri='759209512951.dkr.ecr.us-west-2.amazonaws.com/sagemaker-debugger-rule-evaluator:latest', 
    instance_type='ml.t3.medium',     
    source='path/to/my_custom_rule.py', 
    rule_to_invoke='CustomGradientRule',     
    collections_to_save=[CollectionConfig("gradients")], 
    rule_parameters={"threshold": "20.0"}
)
```

The following list explains the Debugger `Rule.custom` API arguments\.
+ `name` \(str\): Specify a custom rule name as you want\.
+ `image_uri` \(str\): This is the image of the container that has the logic of understanding your custom rule\. It sources and evaluates the specified tensor collections you save in the training job\. You can find the list of open source SageMaker rule evaluator images from [Amazon SageMaker Debugger Registry URLs for Custom Rule Evaluators](debugger-docker-images-rules.md#debuger-custom-rule-registry-ids)\.
+ `instance_type` \(str\): You need to specify an instance to build a rule docker container\. This spins up the instance in parallel with a training container\.
+ `source` \(str\): This is the local path or the Amazon S3 URI to your custom rule script\.
+ `rule_to_invoke` \(str\): This specifies the particular Rule class implementation in your custom rule script\. SageMaker supports only one rule to be evaluated at a time in a rule job\.
+ `collections_to_save` \(str\): This specifies which tensor collections you will save for the rule to run\.
+ `rule_parameters` \(dictionary\): This accepts parameter inputs in a dictionary format\. You can adjust the parameters that you configured in the custom rule script\.

After you set up the `custom_rule` object, you can use it for building a SageMaker estimator for any training jobs\. Specify the `entry_point` to your training script\. You do not need to make any change of your training script\.

```
from sagemaker.tensorflow import TensorFlow

estimator = TensorFlow(
                role=sagemaker.get_execution_role(),
                base_job_name='smdebug-custom-rule-demo-tf-keras',
                entry_point='path/to/your_training_script.py'
                train_instance_type='ml.p2.xlarge'
                ...
                
                # debugger-specific arguments below
                rules = [custom_rule]
)

estimator.fit()
```

For more variations and advanced examples of using Debugger custom rules, see the following example notebooks\.
+ [Monitor your training job with Amazon SageMaker Debugger custom rules](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/tensorflow_keras_custom_rule/tf-keras-custom-rule.ipynb)
+ [PyTorch iterative model pruning of ResNet and AlexNet](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/pytorch_iterative_model_pruning)
+ [Trigger Amazon CloudWatch Events using Debugger Rules to Take an Action Based on Training Status with TensorFlow](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/sagemaker-debugger/tensorflow_action_on_rule)