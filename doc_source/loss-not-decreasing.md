# LossNotDecreasing Rule<a name="loss-not-decreasing"></a>

This rule detects when the loss is not decreasing in value at an adequate rate\. These losses must be scalars\. 

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\. You must specify either the `collection_names` or `tensor_regex` parameter\. If both the parameters are specified, the rule inspects the union of tensors from both sets\.

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the LossNotDecreasing Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: None  | 
| tensor\_regex |  A list of regex patterns that is used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: None  | 
| use\_losses\_collection |  If set to `True`, looks for losses in the collection named "losses" when the collection is present\. **Optional** Valid values: Boolean Default value: `True`  | 
| nym\_steps |  The minimum number of steps after which the rule checks if the loss has decreased\. Rule evaluation happens every `num_steps`\. The rule compares the loss for this step with the loss at a step which is at least `num_steps` behind the current step\. For example, suppose that the loss is being saved every three steps, but `num_steps` is set to 10\. At step 21, loss for step 21 is compared with loss for step 9\. The next step at which loss is checked is step 33, because ten steps after step 21 is step 31, and at step 31 and step 32 loss is not saved\.  **Optional** Valid values: Integer Default value: `10`  | 
| diff\_percent |  The minimum percentage difference by which the loss should decrease between `num_steps`\. **Optional** Valid values: `0.0` < float < `100` Default value: Checks if the loss is decreasing between `num_steps`\.  | 
| mode |  The name of the Debugger mode to query tensor values for rule checking\. If this is not passed, the rule checks in order by default for the `mode.EVAL`, then `mode.TRAIN`, and then `mode.GLOBAL`\.  **Optional** Valid values: String \(`EVAL`, `TRAIN`, or `GLOBAL`\) Default value: `None`  | 