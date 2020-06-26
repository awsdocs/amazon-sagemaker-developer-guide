# UnchangedTensor Rule<a name="unchanged-tensor"></a>

This rule detects whether a tensor is no longer changing across steps\. 

This rule runs the `[numpy\.allclose]( https://docs.scipy.org/doc/numpy/reference/generated/numpy.allclose.html)` method to check if the tensor isn't changing\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\. You must specify either the `collection_names` or `tensor_regex` parameter\. If both the parameters are specified, the rule inspects the union of tensors from both sets\.

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the UnchangedTensor Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: None  | 
| tensor\_regex |  A list of regex patternsused to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: None  | 
| num\_steps |  The number of steps across which the rule checks to determine if the tensor has changed\.  This checks the last `num_steps` that are available\. They don't need to be consecutive\. If `num_steps` is 2, at step s it doesn't necessarily check for s\-1 and s\. If s\-1 isn't available, it checks the last available step along with s\. In that case, it checks the last available step with the current step\. **Optional** Valid values: Integer Default value: `3`  | 
| rtol |  The relative tolerance parameter to be passed to the `[numpy\.allclose]( https://docs.scipy.org/doc/numpy/reference/generated/numpy.allclose.html)` method\.  **Optional** Valid values: Float Default value: `1e-05`  | 
| atol |  The absolute tolerance parameter to be passed to the `[numpy\.allclose]( https://docs.scipy.org/doc/numpy/reference/generated/numpy.allclose.html)` method\. **Optional** Valid values: Float Default value: `1e-08`  | 
| equal\_nan |  Whether to compare NaNs as equal\. If `True`, NaNs in input array a are considered equal to NaNs in input array b in the output array\. This parameter is passed to the `[numpy\.allclose]( https://docs.scipy.org/doc/numpy/reference/generated/numpy.allclose.html)` method\. **Optional** Valid values: Boolean Default value: `False`  | 