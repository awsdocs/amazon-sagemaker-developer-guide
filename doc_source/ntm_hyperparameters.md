# NTM Hyperparameters<a name="ntm_hyperparameters"></a>


| Parameter Name | Description | 
| --- | --- | 
|  `feature_dim`  |  Vocabulary size of the dataset\. Required\. Valid values: Positive integer \(min: 1, max: 1,000,000\) Default value: \-  | 
| num\_topics |  Number of required topics Valid values: Positive integer \(min: 2, max: 1000\) Default value: \-  | 
| encoder\_layers |  Represents the number of layers in the encoder and the output size of each layer\. When set to *auto*, the algorithm uses two layers of sizes 3 x `num_topics` and 2 x `num_topics` respectively\.  Valid values: Comma\-separated list of positive integers or *auto* Default value: *auto*  | 
| mini\_batch\_size |  Number of examples in each mini batch\. Valid values: Positive integer \(min: 1, max: 10000\) Default value: 256  | 
| epochs |  Maximum number of passes over the training data\. Valid values: Positive integer \(min: 1, max: 100\) Default value: 50  | 
| encoder\_layers\_activation |  Activation function to use in the encoder layers\. Valid values: One of *sigmoid*, *tanh*, or *relu* Default value: *sigmoid*  | 
| optimizer |  Optimizer to use for training\. Valid values: One of *sgd*, *adam*, *adagrad* , *adadelta* , or *rmsprop* Default value: *adadelta*  | 
| tolerance |  Maximum relative change in the loss function within the last `num_patience_epochs`  number of epochs below which early stopping is triggered\.  Valid values: float, min: 1e\-6, max: 0\.1 Default value: 0\.001  | 
| num\_patience\_epochs |  Number of successive epochs over which early stopping criterion is evaluated\. Valid values: Positive integer \(min: 1, max: 10\) Default value: 3  | 
| batch\_norm |  Whether to use batch normalization during training\. Valid values: *true* or *false* Default value: *false*  | 
| rescale\_gradient |  Rescale factor for gradient\. Valid values: float, min: 1e\-3, max: 1\.0 Default value: 1\.0  | 
| clip\_gradient |  Maximum magnitude for each gradient component\. Valid values: float, min: 1e\-3 Default value: Infinity  | 
| weight\_decay |   Weight decay coefficient\. Adds L2 regularization\. Valid values: float, min: 0\.0, max: 1\.0 Default value: 0\.0  | 
| learning\_rate |  Learning rate for the optimizer\. Valid values: float, min: 1e\-6, max: 1\.0 Default value: 0\.001  | 