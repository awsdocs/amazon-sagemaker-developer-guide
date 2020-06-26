# PoorWeightInitialization Rule<a name="poor-weight-initialization"></a>

 This rule detects if your model parameters have been poorly initialized\. 

Good initialization breaks the symmetry of the weights and gradients in a neural network and maintains commensurate activation variances across layers\. Otherwise, the neural network doesn't learn effectively\. Initializers like Xavier aim to keep variance constant across activations, which is especially relevant for training very deep neural nets\. Too small an initialization can lead to vanishing gradients\. Too large an initialization can lead to exploding gradients\. This rule checks the variance of activation inputs across layers, the distribution of gradients, and the loss convergence for the initial steps to determine if a neural network has been poorly initialized\.


**Parameter Descriptions for the PoorWeightInitialization Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| activation\_inputs\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string Default value: `".*relu_input"`  | 
| threshold |  If the ratio between minimum and maximum variance of weights per layer exceeds the `threshold` at a step, the rule returns `True`\. **Optional** Valid values: Float Default value: `10.0`  | 
| distribution\_range | If the minimum difference between 5th and 95th percentiles of the gradient distribution is less than the `distribution_range`, the rule returns `True`\. **Optional** Valid values: Float Default value: `0.001`  | 
| patience | The number of steps to wait until the loss is considered to be no longer decreasing\. **Optional** Valid values: Integer Default value: `5`  | 
| steps | The number of steps this rule analyzes\. You typically need to check only the first few iterations\. **Optional** Valid values: Float Default value: `10`  | 

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.

**Note**  
This rule can't be applied to the XGBoost algorithm\.