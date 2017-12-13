# NTM Hyperparameters<a name="ntm_hyperparameters"></a>


| Parameter Name | Description | 
| --- | --- | 
|  `feature_dim`  |  Vocabulary size of the dataset\. Required\. Valid values: Positive integer \(min: 1, max: 1,000,000\) Default values: \-  | 
| num\_topics |  Number of required topics Valid values: Positive integer \(min: 2, max: 1000\) Default values: \-  | 
| encoder\_layers |  Represents the number of layers in the encoder and the output size of each layer\. When set to *auto*, the algorithm uses two layers of sizes 3 x `num_topics` and 2 x `num_topics `respectively\.  Valid values: Comma\-separated list of positive integers or `auto` Default values: auto  | 
| mini\_batch\_size |  Number of examples in each mini batch\. Valid values: Positive integer \(min: 1, max: 10000\) Default values: 256  | 
| epochs |  Maximum number of passes over the training data\. Valid values: Positive integer \(min: 1, max: 100\) Default values: 50  | 
| encoder\_layers\_activation |  Activation function to use in the encoder layers\. Valid values: One of *sigmoid*, *tanh*, or *relu* Default values: *sigmoid*  | 
| optimizer |  Optimizer to use for training\. Valid values: One of *sgd*, *adam*, *adagrad* , *adadelta* , or *rmsprop* Default values: *adadelta*  | 
| tolerance |  Maximum relative change in the loss function within the last `num_patience_epochs`  number of epochs below which early stopping is triggered\.  Valid values: float, min: 1e\-6, max: 0\.1 Default values: 0\.001  | 
| num\_patience\_epochs |  Number of successive epochs over which early stopping criterion is evaluated\. Valid values: Positive integer \(min: 1, max: 10\) Default values: 3  | 
| batch\_norm |  Whether to use batch normalization during training\. Valid values: *true* or *false* Default values: *false*  | 
| rescale\_gradient |  Rescale factor for gradient\. Optional\. Valid values: float, min: 1e\-3, max: 1\.0 Default values: 1\.0  | 
| clip\_gradient |  Maximum magnitude for each gradient component\. Optional\. Valid values: float, min: 1e\-3 Default values: Infinity  | 
| weight\_decay |   Weight decay coefficient\. Adds L2 regularization\. Valid values: float, min: 0\.0, max: 1\.0 Default values: 0\.0  | 
| learning\_rate |  Learning rate for the optimizer\. Valid values: float, min: 1e\-6, max: 1\.0 Default values: 0\.001  | 