# ClassImbalance Rule<a name="class-imbalance"></a>

This rule measures sampling imbalances between classes and throws errors if the imbalance exceeds a threshold or if too many mispredictions for underrepresented classes occur as a result of the imbalance\.

Classification models require well\-balanced classes in the training dataset or a proper weighting/sampling of classes during training\. The rule performs two checks:
+  It counts the occurrences per class\. If the ratio of number of samples between smallest and largest class is larger than the `threshold_imbalance`, an error is thrown\.
+  It checks the prediction accuracy per class\. If resampling or weighting has not been correctly applied, then the model can reach high accuracy for the class with many training samples, but low accuracy for the classes with few training samples\. If a fraction of mispredictions for a certain class is above `threshold_misprediction `, an error is thrown\.

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.


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