# DeadRelu Rule<a name="dead-relu"></a>

This rule detects when the percentage of rectified linear unit \(ReLU\) activation functions in a trial are considered dead because their activation activity has dropped below a threshold\. If the percent of inactive ReLUs in a layer is greater than the `threshold_layer` value of inactive ReLUs, the rule returns `True`\.


**Parameter Descriptions for the DeadRelu Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: `None`  | 
| threshold\_inactivity | Defines a level of activity below which a ReLU is considered to be dead\. A ReLU might be active in the beginning of a trial and then slowly die during the training process\. If the ReLU is active less than the `threshold_inactivity`, it is considered to be dead\. **Optional** Valid values: Float Default values: `1.0`  | 
| threshold\_layer | Returns `True` if the percentage of inactive ReLUs in a layer is greater than `threshold_layer`\. Returns `False` if the percentage of inactive ReLUs in a layer is less than `threshold_layer`\. **Optional** Valid values: Float Default values: `50.0`  | 

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.

**Note**  
This rule can't be applied to the XGBoost algorithm\.