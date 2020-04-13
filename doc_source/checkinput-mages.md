# CheckInputImages Rule<a name="checkinput-mages"></a>

This rule checks if input images have been correctly normalized\. Specifically, it detects if the mean of the sample data differs by more than a threshold value from zero\. Many computer vision models require that input data has a zero mean and unit variance\.

This rule is applicable to deep learning applications\.

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the CheckInputImages Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| threshold\_mean |  A threshold that defines by how much mean of the input data can differ from 0\. **Optional** Valid values: Float Default value: `0.2`  | 
| threshold\_samples |  The number of images that have to be sampled before an error can be thrown\. If the value is too low, the estimation of the dataset mean will be inaccurate\. **Optional** Valid values: Integer Default value: `500`  | 
| regex |  The name of the input data tensor\. **Optional** Valid values: String Default value: `".*hybridsequential0_input_0"` \(the name of the input tensor for Apache MXNet models using HybridSequential\)  | 
| channel |  The position of the color channel in the input tensor\.  **Optional** Valid values: Integer Default value: `1` \(for example, MXNet expects input data in the form of \(batch\_size, channel, height, width\)\)  | 