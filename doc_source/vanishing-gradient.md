# VanishingGradient Rule<a name="vanishing-gradient"></a>

This rule detects if the gradients in a trial become extremely small or drop to a zero magnitude\. If the mean of the absolute values of the gradients drops below a specified `threshold`, the rule returns `True`\.


**Parameters Descriptions for the VanishingGradient Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| threshold | The value at which the gradient is determined to be vanishing\.**Optional**Valid values: FloatDefault value: `0.0000001`\. | 

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.

**Note**  
This rule can't be applied to the XGBoost algorithm\.