# WeightUpdateRatio Rule<a name="weight-update-ratio"></a>

This rule keeps track of the ratio of updates to weights during training and detects if that ratio gets too large or too small\. If the ratio of updates to weights is larger than the `large_threshold value` or if this ratio is smaller than `small_threshold`, the rule returns `True`\.

Conditions for training are best when the updates are commensurate to the gradients\. Excessively large updates can push the weights away from optimal values, and very small updates result in very slow convergence\. This rule requires weights to be available for two consecutive steps, so `save_interval` needs to be set to 1\.


**Parameter Descriptions for the WeightUpdateRatio Rule**  

| Parameter Name, | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| num\_steps |  The number of steps across which the rule checks to determine if the tensor has changed\.  The number of steps across which you want to compare the weight ratios\. If you pass no value, the rule runs by default against the current step and the immediately previous saved step\. If you override the default by passing a value for this parameter, the comparison is done between weights at step s and at a step that is >= s \- `num_steps`\. **Optional** Valid values: Integer Default value: `None`  | 
| large\_threshold |  The maximum value that the ratio of updates to weigh can take before the rule returns `True`\.  **Optional** Valid values: Float Default value: `10.0`  | 
| small\_threshold |  The minimum value that the ratio of updates to weigh can take, below which the rule returns `True`\. **Optional** Valid values: Float Default value: `0.00000001`  | 
| epsilon |  A small constant used to ensure that Debugger does not divide by zero when computing the ratio updates to weigh\. **Optional** Valid values: Float Default value: `0.000000001`  | 

**Note**  
If tensors have been saved with `TRAIN` mode on during training, the rule runs only on `TRAIN` mode steps\. Otherwise, it runs by default on `GLOBAL` mode steps\.

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.

**Note**  
This rule can't be applied to the XGBoost algorithm\.