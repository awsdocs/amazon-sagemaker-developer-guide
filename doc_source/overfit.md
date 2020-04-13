# Overfit Rule<a name="overfit"></a>

This rule detects if your model is being overfit to the training data by comparing the validation and training losses\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.

**Note**  
A standard way to prevent overfitting is to regularize your model\.


**Parameter Descriptions for the Overfit Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| tensor\_regex |  A list of regex patterns that is used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: None  | 
| start\_step |  The step from which to start comparing the validation and training loss\. **Optional** Valid values: Integer Default value: `0`  | 
| patience |  The number of steps for which the `ratio_threshold` is allowed to exceed the value set before the model is considered to be overfit\. **Optional** Valid values: Integer Default value: `1`  | 
| ratio\_threshold |  The maximum ratio of the difference between the mean validation loss and mean training loss to the mean training loss\. If this threshold is exceeded for a `patience` number of steps, the model is being overfit and the rule returns `True`\. **Optional** Valid values: Float Default value: `0.1`  | 