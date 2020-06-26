# ExplodingTensor Rule<a name="exploding-tensor"></a>

This rule detects whether the tensors emitted during training have non\-finite values, either infinite or NaN \(not a number\)\. If a non\-finite value is detected, the rule returns `True`\.


**Parameter Descriptions for the ExplodingTensor Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| collection\_names | The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: `None`  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: `None`  | 
| only\_nan |  `True` to monitor the `base_trial` tensors only for `NaN` values and not for infinity\.  `False` to treat both `NaN` and infinity as exploding values and to monitor for both\. **Optional** Valid values: `False`  | 

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.

**Note**  
This rule can't be applied to the XGBoost algorithm\.