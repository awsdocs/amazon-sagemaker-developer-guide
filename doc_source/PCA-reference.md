# PCA Hyperparameters<a name="PCA-reference"></a>

In the `CreateTrainingJob` request, you specify the training algorithm\. You can also specify algorithm\-specific HyperParameters as string\-to\-string maps\. The following table lists the hyperparameters for the PCA training algorithm provided by Amazon SageMaker\. For more information about how PCA works, see [How PCA Works](how-pca-works.md)\. 


| Parameter Name | Description | 
| --- | --- | 
| feature\_dim |  Input dimension\. **Required** Valid values: positive integer  | 
| mini\_batch\_size |  Number of rows in a mini\-batch\. **Required** Valid values: positive integer  | 
| num\_components |  The number of principal components to compute\. **Required** Valid values: positive integer  | 
| algorithm\_mode |  Mode for computing the principal components\.  **Optional** Valid values: *regular* or *randomized* Default value: *regular*  | 
| extra\_components |  As the value increases, the solution becomes more accurate but the runtime and memory consumption increase linearly\. The default, \-1, means the maximum of 10 and `num_components`\. Valid for *randomized* mode only\. **Optional** Valid values: Non\-negative integer or \-1 Default value: \-1  | 
| subtract\_mean |  Indicates whether the data should be unbiased both during training and at inference\.  **Optional** Valid values: One of *true* or *false* Default value: *true*  | 