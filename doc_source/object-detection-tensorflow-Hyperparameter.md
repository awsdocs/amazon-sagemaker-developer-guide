# Object Detection \- TensorFlow Hyperparameters<a name="object-detection-tensorflow-Hyperparameter"></a>

Hyperparameters are parameters that are set before a machine learning model begins learning\. The following hyperparameters are supported by the Amazon SageMaker built\-in Object Detection \- TensorFlow algorithm\. See [Tune an Object Detection \- TensorFlow model](object-detection-tensorflow-tuning.md) for information on hyperparameter tuning\. 


| Parameter Name | Description | 
| --- | --- | 
| batch\_size |  The batch size for training\.  Valid values: positive integer\. Default value: `3`\.  | 
| beta\_1 |  The beta1 for the `"adam"` optimizer\. Represents the exponential decay rate for the first moment estimates\. Ignored for other optimizers\. Valid values: float, range: \[`0.0`, `1.0`\]\. Default value: `0.9`\.  | 
| beta\_2 |  The beta2 for the `"adam"` optimizer\. Represents the exponential decay rate for the second moment estimates\. Ignored for other optimizers\. Valid values: float, range: \[`0.0`, `1.0`\]\. Default value: `0.999`\.  | 
| early\_stopping |  Set to `"True"` to use early stopping logic during training\. If `"False"`, early stopping is not used\. Valid values: string, either: \(`"True"` or `"False"`\)\. Default value: `"False"`\.  | 
| early\_stopping\_min\_delta | The minimum change needed to qualify as an improvement\. An absolute change less than the value of early\_stopping\_min\_delta does not qualify as improvement\. Used only when early\_stopping is set to "True"\.Valid values: float, range: \[`0.0`, `1.0`\]\.Default value: `0.0`\. | 
| early\_stopping\_patience |  The number of epochs to continue training with no improvement\. Used only when `early_stopping` is set to `"True"`\. Valid values: positive integer\. Default value: `5`\.  | 
| epochs |  The number of training epochs\. Valid values: positive integer\. Default value: `5` for smaller models, `1` for larger models\.  | 
| epsilon |  The epsilon for `"adam"`, `"rmsprop"`, `"adadelta"`, and `"adagrad"` optimizers\. Usually set to a small value to avoid division by 0\. Ignored for other optimizers\. Valid values: float, range: \[`0.0`, `1.0`\]\. Default value: `1e-7`\.  | 
| initial\_accumulator\_value |  The starting value for the accumulators, or the per\-parameter momentum values, for the `"adagrad"` optimizer\. Ignored for other optimizers\. Valid values: float, range: \[`0.0`, `1.0`\]\. Default value: `0.1`\.  | 
| learning\_rate | The optimizer learning rate\. Valid values: float, range: \[`0.0`, `1.0`\]\.Default value: `0.001`\. | 
| momentum |  The momentum for the `"sgd"` and `"nesterov"` optimizers\. Ignored for other optimizers\. Valid values: float, range: \[`0.0`, `1.0`\]\. Default value: `0.9`\.  | 
| optimizer |  The optimizer type\. For more information, see [Optimizers](https://www.tensorflow.org/api_docs/python/tf/keras/optimizers) in the TensorFlow documentation\. Valid values: string, any of the following: \(`"adam"`, `"sgd"`, `"nesterov"`, `"rmsprop"`,` "adagrad"` , `"adadelta"`\)\. Default value: `"adam"`\.  | 
| reinitialize\_top\_layer |  If set to `"Auto"`, the top classification layer parameters are re\-initialized during fine\-tuning\. For incremental training, top classification layer parameters are not re\-initialized unless set to `"True"`\. Valid values: string, any of the following: \(`"Auto"`, `"True"` or `"False"`\)\. Default value: `"Auto"`\.  | 
| rho |  The discounting factor for the gradient of the `"adadelta"` and `"rmsprop"` optimizers\. Ignored for other optimizers\.  Valid values: float, range: \[`0.0`, `1.0`\]\. Default value: `0.95`\.  | 
| train\_only\_on\_top\_layer |  If `"True"`, only the top classification layer parameters are fine\-tuned\. If `"False"`, all model parameters are fine\-tuned\. Valid values: string, either: \(`"True"` or `"False"`\)\. Default value: `"False"`\.  | 