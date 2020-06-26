# SaturatedActivation Rule<a name="saturated-activation"></a>

This rule detects if the tanh and sigmoid activation layers are becoming saturated\. An activation layer is saturated when the input of the layer is close to the maximum or minimum of the activation function\. The minimum and maximum of the tanh and sigmoid activation functions are defined by their respective `min_threshold` and `max_thresholds` values\. If the activity of a node drops below the `threshold_inactivity` percentage, it is considered saturated\. If more than a `threshold_layer` percent of the nodes are saturated, the rule returns `True`\.


**Parameter Descriptions for the SaturatedActivation Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| collection\_names | The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: None  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: `".*tanh_input, .*sigmoid_input".`  | 
| threshold\_tanh | The minimum and maximum thresholds that define the extremes of the input for a tanh activation function, defined as: `(min_threshold, max_threshold)`\. The default values are determined based on a vanishing gradient threshold of 0\.0000001\. **Optional** Valid values: Float Default values: `(-9.4999, 9.4999)`  | 
| threshold\_sigmoid | The minimum and maximum thresholds that define the extremes of the input for a sigmoid activation function, defined as: `(min_threshold, max_threshold)`\. The default values are determined based on a vanishing gradient threshold of 0\.0000001\. **Optional** Valid values: Float Default values: `(-23, 16.99999)`  | 
| threshold\_inactivity | The percentage of inactivity below which the activation layer is considered to be saturated\. The activation might be active in the beginning of a trial and then slowly become less active during the training process\. **Optional** Valid values: Float Default values: `1.0`  | 
| threshold\_layer | Returns `True` if the number of saturated activations in a layer is greater than the `threshold_layer` percentage\. Returns `False` if the number of saturated activations in a layer is less than the `threshold_layer` percentage\. **Optional** Valid values: Float Default values: `50.0`  | 

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.

**Note**  
This rule can't be applied to the XGBoost algorithm\.