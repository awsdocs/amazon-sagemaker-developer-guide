# Overtraining Rule<a name="overtraining"></a>

This rule detects if the model is being overtrained\. 

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.

**Note**  
Overtraining can be avoided by early stopping\. For information on early stopping, see [Stop Training Jobs Early](automatic-model-tuning-early-stopping.md)\. For an example that shows how to use spot training with Debugger, see [Enable Spot Training with Amazon SageMaker Debugger](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/mxnet_spot_training/mxnet-spot-training-with-sagemakerdebugger.ipynb)\. 


**Parameter Descriptions for the Overtraining Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| patience\_train | The number of steps to wait before the training loss is considered to not to be improving anymore\. **Optional** Valid values: Integer Default value: `5`  | 
| patience\_validation | The number of steps to wait before the validation loss is considered to not to be improving anymore\.**Optional**Valid values: IntegerDefault value: `10` | 
| delta | The minimum threshold by how much the error should improve before it is considered as a new optimum\. **Optional** Valid values: Float Default value: `0.1`  | 

The following Python example shows how to implement this rule\.

```
rules_specification = [
    {
        "RuleName": "Overtraining",
        "InstanceType": "ml.c5.4xlarge",
        "RuntimeConfigurations": {
            "patience_train" : "10",
            "patience_validation": "20"
        }
    }
]
```