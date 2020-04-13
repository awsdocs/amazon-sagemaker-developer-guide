# NTM Hyperparameters<a name="ntm_hyperparameters"></a>


| Parameter Name | Description | 
| --- | --- | 
|  `feature_dim`  |  The vocabulary size of the dataset\. **Required** Valid values: Positive integer \(min: 1, max: 1,000,000\)  | 
| num\_topics |  The number of required topics\. **Required** Valid values: Positive integer \(min: 2, max: 1000\)  | 
| batch\_norm |  Whether to use batch normalization during training\. **Optional** Valid values: *true* or *false* Default value: *false*  | 
| clip\_gradient |  The maximum magnitude for each gradient component\. **Optional** Valid values: Float \(min: 1e\-3\) Default value: Infinity  | 
| encoder\_layers |  The number of layers in the encoder and the output size of each layer\. When set to *auto*, the algorithm uses two layers of sizes 3 x `num_topics` and 2 x `num_topics` respectively\.  **Optional** Valid values: Comma\-separated list of positive integers or *auto* Default value: *auto*  | 
| encoder\_layers\_activation |  The activation function to use in the encoder layers\. **Optional** Valid values:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/ntm_hyperparameters.html) Default value: `sigmoid`  | 
| epochs |  The maximum number of passes over the training data\. **Optional** Valid values: Positive integer \(min: 1\) Default value: 50  | 
| learning\_rate |  The learning rate for the optimizer\. **Optional** Valid values: Float \(min: 1e\-6, max: 1\.0\) Default value: 0\.001  | 
| mini\_batch\_size |  The number of examples in each mini batch\. **Optional** Valid values: Positive integer \(min: 1, max: 10000\) Default value: 256  | 
| num\_patience\_epochs |  The number of successive epochs over which early stopping criterion is evaluated\. Early stopping is triggered when the change in the loss function drops below the specified `tolerance` within the last `num_patience_epochs` number of epochs\. To disable early stopping, set `num_patience_epochs` to a value larger than `epochs`\. **Optional** Valid values: Positive integer \(min: 1\) Default value: 3  | 
| optimizer |  The optimizer to use for training\. **Optional** Valid values: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/ntm_hyperparameters.html) Default value: `adadelta`  | 
| rescale\_gradient |  The rescale factor for gradient\. **Optional** Valid values: float \(min: 1e\-3, max: 1\.0\) Default value: 1\.0  | 
| sub\_sample |  The fraction of the training data to sample for training per epoch\. **Optional** Valid values: Float \(min: 0\.0, max: 1\.0\) Default value: 1\.0  | 
| tolerance |  The maximum relative change in the loss function\. Early stopping is triggered when change in the loss function drops below this value within the last `num_patience_epochs` number of epochs\. **Optional** Valid values: Float \(min: 1e\-6, max: 0\.1\) Default value: 0\.001  | 
| weight\_decay |   The weight decay coefficient\. Adds L2 regularization\. **Optional** Valid values: Float \(min: 0\.0, max: 1\.0\) Default value: 0\.0  | 