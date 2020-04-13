# Confusion Rule<a name="confusion"></a>

This rule evaluates the goodness of a confusion matrix for a classification problem\.

It creates a matrix of size `category_no*category_no` and populates it with data coming from \(`labels`, `predictions`\) pairs\. For each \(`labels`, `predictions`\) pair, the count in `confusion[labels][predictions]` is incremented by 1\. When the matrix is fully populated, the ratio of data on\-diagonal values and off\-diagonal values are evaluated as follows:
+ For elements on the diagonal: `confusion[i][i]/sum_j(confusion[j][j])>=min_diag`
+ For elements off the diagonal: `confusion[j][i])/sum_j(confusion[j][i])<=max_off_diag`

This rule can be applied either to one of the supported deep learning frameworks \(TensorFlow, MXNet, and PyTorch\) or to the XGBoost algorithm\.

For an example of how to configure and deploy a built\-in rule, see [How to Use Built\-in Rules for Model Analysis](use-debugger-built-in-rules.md)\.


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