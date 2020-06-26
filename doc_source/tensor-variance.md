# TensorVariance Rule<a name="tensor-variance"></a>

This rule detects if you have tensors with very high or low variances\. Very high or low variances in a tensor could lead to neuron saturation, which reduces the learning ability of the neural network\. Very high variance in tensors can also eventually lead to exploding tensors\. Use this rule to detect such issues early\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\. You must specify either the `collection_names` or `tensor_regex` parameter\. If both the parameters are specified, the rule inspects the union of tensors from both sets\.

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the TensorVariance Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| collection\_names | The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: None  | 
| tensor\_regex | A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: None  | 
| max\_threshold | The threshold for the upper bound of tensor variance\. **Optional** Valid values: Float Default value: `xxx`  | 
| min\_threshold | The threshold for the lower bound of tensor variance\. **Optional** Valid values: Float Default value: `xxx`  | 