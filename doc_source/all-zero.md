# AllZero Rule<a name="all-zero"></a>

This rule detects if all or a specified percentage of the values in the tensors are zero\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\. You must specify either the `collection_names` or `tensor_regex` parameter\. If both the parameters are specified, the rule inspects the union of tensors from both sets\.

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.


**Parameters Descriptions for the AllZero Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: `None`  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string Default value: `None`  | 
| threshold |  Specifies the percentage of values in the tensor that needs to be zero for this rule to be invoked\.  **Optional** Valid values: Float Default value: 100  | 