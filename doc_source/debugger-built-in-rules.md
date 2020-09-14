# List of Debugger Built\-in Rules<a name="debugger-built-in-rules"></a>

Use the Debugger built\-in rules provided by Amazon SageMaker Debugger and analyze tensors emitted while training your models\. The Debugger built\-in rules monitor various common conditions that are critical for the success of a training job\. You can call the built\-in rules using Amazon SageMaker Python SDK or the low\-level SageMaker API operations\. Depending on deep learning frameworks of your choice, there are four scopes of validity for the built\-in rules as shown in the following table\.

**Note**  
The number of rules you can run in parallel run on `ml.t3.medium` instances\. The maximum number of built\-in rule containers for a training job is 20\. You cannot specify other instance types for the Debugger built\-in rules\.


**Scopes of Validity for the Debugger Built\-in Rules**  

| Scope of Validity | Built\-in Rules \(SageMaker Python SDK format\) | Built\-in Rules \(SageMaker API format\) | 
| --- | --- | --- | 
| Deep learning frameworks \(TensorFlow, Apache MXNet, and PyTorch\) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) | 
| Deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) and the XGBoost algorithm  | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) | 
| Deep learning applications | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) | 
| XGBoost algorithm | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/debugger-built-in-rules.html) | 
+ For a full instruction of using the built\-in rules with SageMaker Python SDK, see [Use Debugger Built\-in Rules](use-debugger-built-in-rules.md#debugger-deploy-built-in-rules)\.
+ For a full instruction of using the built\-in rules with the low\-level SageMaker API operations, see [Add Debugger Built\-in Rule Configuration to the `CreateTrainingJob` API Operation](debugger-createtrainingjob-api.md#debugger-built-in-rules-api)\.

## DeadRelu<a name="dead-relu"></a>

This rule detects when the percentage of rectified linear unit \(ReLU\) activation functions in a trial are considered dead because their activation activity has dropped below a threshold\. If the percent of inactive ReLUs in a layer is greater than the `threshold_layer` value of inactive ReLUs, the rule returns `True`\.


**Parameter Descriptions for the DeadRelu Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: `None`  | 
| threshold\_inactivity | Defines a level of activity below which a ReLU is considered to be dead\. A ReLU might be active in the beginning of a trial and then slowly die during the training process\. If the ReLU is active less than the `threshold_inactivity`, it is considered to be dead\. **Optional** Valid values: Float Default values: `1.0`  | 
| threshold\_layer | Returns `True` if the percentage of inactive ReLUs in a layer is greater than `threshold_layer`\. Returns `False` if the percentage of inactive ReLUs in a layer is less than `threshold_layer`\. **Optional** Valid values: Float Default values: `50.0`  | 

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.

**Note**  
This rule is not available for the XGBoost algorithm\.

## ExplodingTensor<a name="exploding-tensor"></a>

This rule detects whether the tensors emitted during training have non\-finite values, either infinite or NaN \(not a number\)\. If a non\-finite value is detected, the rule returns `True`\.


**Parameter Descriptions for the ExplodingTensor Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| collection\_names | The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: `None`  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: `None`  | 
| only\_nan |  `True` to monitor the `base_trial` tensors only for `NaN` values and not for infinity\.  `False` to treat both `NaN` and infinity as exploding values and to monitor for both\. **Optional** Valid values: `False`  | 

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.

**Note**  
This rule is not available for the XGBoost algorithm\.

## PoorWeightInitialization<a name="poor-weight-initialization"></a>

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

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.

**Note**  
This rule is not available for the XGBoost algorithm\.

## SaturatedActivation<a name="saturated-activation"></a>

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

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.

**Note**  
This rule is not available for the XGBoost algorithm\.

## VanishingGradient<a name="vanishing-gradient"></a>

This rule detects if the gradients in a trial become extremely small or drop to a zero magnitude\. If the mean of the absolute values of the gradients drops below a specified `threshold`, the rule returns `True`\.


**Parameters Descriptions for the VanishingGradient Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| threshold | The value at which the gradient is determined to be vanishing\.**Optional**Valid values: FloatDefault value: `0.0000001`\. | 

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.

**Note**  
This rule is not available for the XGBoost algorithm\.

## WeightUpdateRatio<a name="weight-update-ratio"></a>

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

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.

**Note**  
This rule is not available for the XGBoost algorithm\.

## AllZero<a name="all-zero"></a>

This rule detects if all or a specified percentage of the values in the tensors are zero\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\. You must specify either the `collection_names` or `tensor_regex` parameter\. If both the parameters are specified, the rule inspects the union of tensors from both sets\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.


**Parameters Descriptions for the AllZero Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: `None`  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string Default value: `None`  | 
| threshold |  Specifies the percentage of values in the tensor that needs to be zero for this rule to be invoked\.  **Optional** Valid values: Float Default value: 100  | 

## ClassImbalance<a name="class-imbalance"></a>

This rule measures sampling imbalances between classes and throws errors if the imbalance exceeds a threshold or if too many mispredictions for underrepresented classes occur as a result of the imbalance\.

Classification models require well\-balanced classes in the training dataset or a proper weighting/sampling of classes during training\. The rule performs the following checks:
+  It counts the occurrences per class\. If the ratio of number of samples between smallest and largest class is larger than the `threshold_imbalance`, an error is thrown\.
+  It checks the prediction accuracy per class\. If resampling or weighting has not been correctly applied, then the model can reach high accuracy for the class with many training samples, but low accuracy for the classes with few training samples\. If a fraction of mispredictions for a certain class is above `threshold_misprediction`, an error is thrown\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the ClassImbalance Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| threshold\_imbalance |  The acceptable imbalance between the number of samples in the smallest class and in the largest class\. Exceeding this threshold value throws an error\. **Optional** Valid values: Float Default value: `10`  | 
| threshold\_misprediction |  A limit on the fraction of mispredictions allowed for each class\. Exceeding this threshold throws an error\. The underrepresented classes are most at risk of crossing this threshold\.  **Optional** Valid values: Float Default value: `0.7`  | 
| samples |  The number of labels that have to be processed before an imbalance is evaluated\. The rule might not be triggered until it has seen sufficient samples across several steps\. The more classes that your dataset contains, the larger this `sample` number should be\.  **Optional** Valid values: Integer Default value: `500` \(assuming a dataset like MNIST with 10 classes\)  | 
| argmax |  If `True`, [np\.argmax]( https://docs.scipy.org/doc/numpy-1.9.3/reference/generated/numpy.argmax.html) is applied to the prediction tensor\. Required when you have a vector of probabilities for each class\. It is used to determine which class has the highest probability\. **Conditional** Valid values: Boolean Default value: `False`  | 
| labels\_regex |  The name of the tensor that contains the labels\. **Optional** Valid values: String Default value: `".*labels"`  | 
| predictions\_regex |  The name of the tensor that contains the predictions\. **Optional** Valid values: String Default value: `".*predictions"`  | 

## Confusion<a name="confusion"></a>

This rule evaluates the goodness of a confusion matrix for a classification problem\.

It creates a matrix of size `category_no*category_no` and populates it with data coming from \(`labels`, `predictions`\) pairs\. For each \(`labels`, `predictions`\) pair, the count in `confusion[labels][predictions]` is incremented by 1\. When the matrix is fully populated, the ratio of data on\-diagonal values and off\-diagonal values are evaluated as follows:
+ For elements on the diagonal: `confusion[i][i]/sum_j(confusion[j][j])>=min_diag`
+ For elements off the diagonal: `confusion[j][i])/sum_j(confusion[j][i])<=max_off_diag`

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the Confusion Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| category\_no |   The number of categories\. **Optional** Valid values: Integer ≥2 Default value: The larger of the number of categories in `labels` and `predictions`\.  | 
| labels |  The tensor name for 1\-d vector of true labels\. **Optional** Valid values: String Default value: `"labels"`  | 
| predictions |  The tensor name for 1\-d vector of estimated labels\.  **Optional** Valid values: String Default value: `"predictions"`  | 
| labels\_collection |  The rule inspects the tensors in this collection for `labels`\. **Optional** Valid values: String Default value: `"labels"`  | 
| predictions\_collection |  The rule inspects the tensors in this collection for `predictions`\. **Optional** Valid values: String Default value: `"predictions"`  | 
| min\_diag |  The minimum value for the ratio of data on the diagonal\. **Optional** Valid values: `0`≤float≤`1` Default value: `0.9`  | 
| max\_off\_diag |  The maximum value for the ratio of data off the diagonal\. **Optional** Valid values: `0`≤float≤`1` Default value: `0.1`  | 

**Note**  
This rule infers default values for the optional parameters if their values aren't specified\.

## LossNotDecreasing<a name="loss-not-decreasing"></a>

This rule detects when the loss is not decreasing in value at an adequate rate\. These losses must be scalars\. 

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\. You must specify either the `collection_names` or `tensor_regex` parameter\. If both the parameters are specified, the rule inspects the union of tensors from both sets\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the LossNotDecreasing Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: None  | 
| tensor\_regex |  A list of regex patterns that is used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: None  | 
| use\_losses\_collection |  If set to `True`, looks for losses in the collection named "losses" when the collection is present\. **Optional** Valid values: Boolean Default value: `True`  | 
| num\_steps |  The minimum number of steps after which the rule checks if the loss has decreased\. Rule evaluation happens every `num_steps`\. The rule compares the loss for this step with the loss at a step which is at least `num_steps` behind the current step\. For example, suppose that the loss is being saved every three steps, but `num_steps` is set to 10\. At step 21, loss for step 21 is compared with loss for step 9\. The next step at which loss is checked is step 33, because ten steps after step 21 is step 31, and at step 31 and step 32 loss is not saved\.  **Optional** Valid values: Integer Default value: `10`  | 
| diff\_percent |  The minimum percentage difference by which the loss should decrease between `num_steps`\. **Optional** Valid values: `0.0` < float < `100` Default value: Checks if the loss is decreasing between `num_steps`\.  | 
| mode |  The name of the Debugger mode to query tensor values for rule checking\. If this is not passed, the rule checks in order by default for the `mode.EVAL`, then `mode.TRAIN`, and then `mode.GLOBAL`\.  **Optional** Valid values: String \(`EVAL`, `TRAIN`, or `GLOBAL`\) Default value: `None`  | 

## Overfit<a name="overfit"></a>

This rule detects if your model is being overfit to the training data by comparing the validation and training losses\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.

**Note**  
A standard way to prevent overfitting is to regularize your model\.


**Parameter Descriptions for the Overfit Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: None  | 
| start\_step |  The step from which to start comparing the validation and training loss\. **Optional** Valid values: Integer Default value: `0`  | 
| patience |  The number of steps for which the `ratio_threshold` is allowed to exceed the value set before the model is considered to be overfit\. **Optional** Valid values: Integer Default value: `1`  | 
| ratio\_threshold |  The maximum ratio of the difference between the mean validation loss and mean training loss to the mean training loss\. If this threshold is exceeded for a `patience` number of steps, the model is being overfit and the rule returns `True`\. **Optional** Valid values: Float Default value: `0.1`  | 

## Overtraining<a name="overtraining"></a>

This rule detects if the model is being overtrained\. 

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.

**Note**  
Overtraining can be avoided by early stopping\. For information on early stopping, see [Stop Training Jobs Early](automatic-model-tuning-early-stopping.md)\. For an example that shows how to use spot training with Debugger, see [Enable Spot Training with Amazon SageMaker Debugger](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/mxnet_spot_training/mxnet-spot-training-with-sagemakerdebugger.ipynb)\. 


**Parameter Descriptions for the Overtraining Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| patience\_train | The number of steps to wait before the training loss is considered to not to be improving anymore\. **Optional** Valid values: Integer Default value: `5`  | 
| patience\_validation | The number of steps to wait before the validation loss is considered to not to be improving anymore\.**Optional**Valid values: IntegerDefault value: `10` | 
| delta | The minimum threshold by how much the error should improve before it is considered as a new optimum\. **Optional** Valid values: Float Default value: `0.1`  | 

The following Python example shows how to implement this rule\.

```
rules_specification = [
    {
        "RuleName": "Overtraining",
        "InstanceType": "ml.c5.4xlarge",
        "RuntimeConfigurations": {
            "patience_train" : "10",
            "patience_validation": "20"
        }
    }
]
```

## SimilarAcrossRuns<a name="similar-across-runs"></a>

This rule compares tensors gathered from a base trial with tensors from another trial\. 

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the SimilarAcrossRuns Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| other\_trial |  The trial whose tensors you want to compare to those tensors gathered from the `base_trial`\. **Required** Valid values: String  | 
| collection\_names |  The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: None  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: None  | 

## StalledTrainingRule<a name="stalled-training"></a>

StalledTrainingRule detects if there is no progress made on training job, and stops the training job if the rule fires\. This rule requires tensors to be periodically saved in a time interval defined by its `threshold` parameter\. This rule keeps on monitoring for new tensors, and if no new tensor has been emitted for threshold interval rule gets fired\. 


**Parameter Descriptions for the CheckInputImages Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| threshold |  A threshold that defines by how much time in seconds the rule waits for a tensor output until it fires a stalled training issue\. Default value is 1800 seconds\. **Optional** Valid values: Integer Default value: `1800`  | 
| stop\_training\_on\_fire |  If set to `True`, watches if the base training job outputs tensors in "`threshold`" seconds\. **Optional** Valid values: Boolean Default value: `False`  | 
| training\_job\_name\_prefix |  The prefix of base training job name\. If `stop_training_on_fire` is true, the rule searches for SageMaker training jobs with this prefix in the same account\. If there is an inactivity found, the rule takes a `StopTrainingJob` action\. Note if there are multiple jobs found with same prefix, the rule skips termination\. It is important that the prefix is set unique per each training job\. **Optional** Valid values: String  | 

## TensorVariance<a name="tensor-variance"></a>

This rule detects if you have tensors with very high or low variances\. Very high or low variances in a tensor could lead to neuron saturation, which reduces the learning ability of the neural network\. Very high variance in tensors can also eventually lead to exploding tensors\. Use this rule to detect such issues early\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\. You must specify either the `collection_names` or `tensor_regex` parameter\. If both the parameters are specified, the rule inspects the union of tensors from both sets\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the TensorVariance Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial | The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| collection\_names | The list of collection names whose tensors the rule inspects\. **Optional** Valid values: List of strings or a comma\-separated string Default value: None  | 
| tensor\_regex | A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: None  | 
| max\_threshold |  The threshold for the upper bound of tensor variance\. **Optional** Valid values: Float Default value: None  | 
| min\_threshold | The threshold for the lower bound of tensor variance\. **Optional** Valid values: Float Default value: None  | 

## UnchangedTensor<a name="unchanged-tensor"></a>

This rule detects whether a tensor is no longer changing across steps\. 

This rule runs the [numpy\.allclose]( https://docs.scipy.org/doc/numpy/reference/generated/numpy.allclose.html) method to check if the tensor isn't changing\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\. You must specify either the `collection_names` or `tensor_regex` parameter\. If both the parameters are specified, the rule inspects the union of tensors from both sets\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.


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

## CheckInputImages<a name="checkinput-mages"></a>

This rule checks if input images have been correctly normalized\. Specifically, it detects if the mean of the sample data differs by more than a threshold value from zero\. Many computer vision models require that input data has a zero mean and unit variance\.

This rule is applicable to deep learning applications\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the CheckInputImages Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| threshold\_mean |  A threshold that defines by how much mean of the input data can differ from 0\. **Optional** Valid values: Float Default value: `0.2`  | 
| threshold\_samples |  The number of images that have to be sampled before an error can be thrown\. If the value is too low, the estimation of the dataset mean will be inaccurate\. **Optional** Valid values: Integer Default value: `500`  | 
| regex |  The name of the input data tensor\. **Optional** Valid values: String Default value: `".*hybridsequential0_input_0"` \(the name of the input tensor for Apache MXNet models using HybridSequential\)  | 
| channel |  The position of the color channel in the input tensor\.  **Optional** Valid values: Integer Default value: `1` \(for example, MXNet expects input data in the form of \(batch\_size, channel, height, width\)\)  | 

## NLPSequenceRatio<a name="nlp-sequence-ratio"></a>

This rule calculates the ratio of specific tokens given the rest of the input sequence that is useful for optimizing performance\. For example, you can calculate the percentage of padding end\-of\-sentence \(EOS\) tokens in your input sequence\. If the number of EOS tokens is too high, an alternate bucketing strategy should be performed\. You also can calculate the percentage of unknown tokens in your input sequence\. If the number of unknown words is too high, an alternate vocabulary could be used\.

This rule is applicable to deep learning applications\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the NLPSequenceRatio Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| tensor\_regex |  A list of regex patterns used to restrict this comparison to specific scalar\-valued tensors\. The rule inspects only the tensors that match the regex patterns specified in the list\. If no patterns are passed, the rule compares all tensors gathered in the trials by default\. Only scalar\-valued tensors can be matched\. **Optional** Valid values: List of strings or a comma\-separated string  Default value: `".*embedding0_input_0"` \(assuming an embedding as the initial layer of the network\)  | 
| token\_values |  A string of a list of the numerical values of the tokens\. For example, "3, 0"\. **Optional** Valid values: Comma\-separated string of numerical values Default value: `0`  | 
| token\_thresholds\_percent |  A string of a list of thresholds \(in percentages\) that correspond to each of the `token_values`\. For example,"50\.0, 50\.0"\. **Optional** Valid values: Comma\-separated string of floats Default value: `"50,50`  | 

## FeatureImportanceOverweight<a name="feature_importance_overweight"></a>

This rule accumulates the weights of the n largest feature importance values per step and ensures that they do not exceed the threshold\. For example, you can set the threshold for the top 3 features to not hold more than 80 percent of the total weights of the model\.

This rule is valid only for the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the TreeDepth Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| threshold |  Defines the threshold for the proportion of the cumulative sum of the `n` largest features\. The number `n` is defined by the `nfeatures` parameter\. **Optional** Valid values: Float Default value: `0.8`  | 
| nfeatures |  The number of largest features\. **Optional** Valid values: Integer Default value: `3`  | 
| tensor\_regex |  Regular expression \(regex\) of tensor names the rule to analyze\. **Optional** Valid values: String Default value: `".*feature_importance/weight"`  | 

## TreeDepth<a name="tree-depth"></a>

This rule measures the depth of trees in an XGBoost model\. XGBoost rejects splits if they do not improve loss\. This regularizes the training\. As a result, the tree might not grow as deep as defined by the `depth` parameter\.

This rule is valid only for the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [Use Debugger Built\-in Rules for Training Job Analysis](use-debugger-built-in-rules.md)\.


**Parameter Descriptions for the TreeDepth Rule**  

| Parameter Name | Description | 
| --- | --- | 
| base\_trial |  The trial run using this rule\. The rule inspects the tensors gathered from this trial\. **Required** Valid values: String  | 
| depth |  The depth of the tree\. The depth of the tree is obtained by computing the base 2 logarithm of the largest node ID\. **Optional** Valid values: Float Default value: `4`  | 