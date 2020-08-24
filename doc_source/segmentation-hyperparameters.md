# Semantic Segmentation Hyperparameters<a name="segmentation-hyperparameters"></a>

The following tables list the hyperparameters supported by the Amazon SageMaker semantic segmentation algorithm for network architecture, data inputs, and training\. You specify Semantic Segmentation for training in the `AlgorithmName` of the [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html) request\.

**Network Architecture Hyperparameters**


| Parameter Name | Description | 
| --- | --- | 
| backbone |  The backbone to use for the algorithm's encoder component\. **Optional** Valid values: `resnet-50`, `resnet-101`  Default value: `resnet-50`  | 
| use\_pretrained\_model |  Whether a pretrained model is to be used for the backbone\. **Optional** Valid values: `True`, `False` Default value: `True`  | 
| algorithm |  The algorithm to use for semantic segmentation\.  **Optional** Valid values: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/segmentation-hyperparameters.html) Default value: `fcn`  | 

**Data Hyperparameters**


| Parameter Name | Description | 
| --- | --- | 
| num\_classes |  The number of classes to segment\. **Required** Valid values: 2 ≤ positive integer ≤ 254  | 
| num\_training\_samples |  The number of samples in the training data\. The algorithm uses this value to set up the learning rate scheduler\. **Required** Valid values: positive integer  | 
| base\_size |  Defines how images are rescaled before cropping\. Images are rescaled such that the long size length is set to `base_size` multiplied by a random number from 0\.5 to 2\.0, and the short size is computed to preserve the aspect ratio\. **Optional** Valid values: positive integer > 16 Default value: 520  | 
| crop\_size |  The image size for input during training\. We randomly rescale the input image based on `base_size`, and then take a random square crop with side length equal to `crop_size`\. The `crop_size` will be automatically rounded up to multiples of 8\. **Optional** Valid values: positive integer > 16 Default value: 480  | 

**Training Hyperparameters**


| Parameter Name | Description | 
| --- | --- | 
| early\_stopping |  Whether to use early stopping logic during training\. **Optional** Valid values: `True`, `False` Default value: `False`  | 
| early\_stopping\_min\_epochs |  The minimum number of epochs that must be run\. **Optional** Valid values: integer Default value: 5  | 
| early\_stopping\_patience |  The number of epochs that meet the tolerance for lower performance before the algorithm enforces an early stop\. **Optional** Valid values: integer Default value: 4  | 
| early\_stopping\_tolerance |  If the relative improvement of the score of the training job, the mIOU, is smaller than this value, early stopping considers the epoch as not improved\. This is used only when `early_stopping` = `True`\. **Optional** Valid values: 0 ≤ float ≤ 1 Default value: 0\.0  | 
| epochs |  The number of epochs with which to train\. **Optional** Valid values: positive integer Default value: 30  | 
| gamma1 |  The decay factor for the moving average of the squared gradient for `rmsprop`\. Used only for `rmsprop`\. **Optional** Valid values: 0 ≤ float ≤ 1 Default value: 0\.9  | 
| gamma2 |  The momentum factor for `rmsprop`\. **Optional** Valid values: 0 ≤ float ≤ 1 Default value: 0\.9  | 
| learning\_rate |  The initial learning rate\.  **Optional** Valid values: 0 < float ≤ 1 Default value: 0\.001  | 
| lr\_scheduler |  The shape of the learning rate schedule that controls its decrease over time\. **Optional** Valid values:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/segmentation-hyperparameters.html) Default value: `poly`  | 
| mini\_batch\_size |  The batch size for training\. Using a large `mini_batch_size` usually results in faster training, but it might cause you to run out of memory\. Memory usage is affected by the values of the `mini_batch_size` and `image_shape` parameters, and the backbone architecture\. **Optional** Valid values: positive integer  Default value: 16  | 
| momentum |  The momentum for the `sgd` optimizer\. When you use other optimizers, the semantic segmentation algorithm ignores this parameter\. **Optional** Valid values: 0 < float ≤ 1 Default value: 0\.9  | 
| optimizer |  The type of optimizer\. For more information about an optimizer, choose the appropriate link: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/segmentation-hyperparameters.html) **Optional** Valid values: `adam`, `adagrad`, `nag`, `rmsprop`, `sgd`  Default value: `sgd`  | 
| syncbn |  If set to `True`, the batch normalization mean and variance are computed over all the samples processed across the GPUs\. **Optional**  Valid values: `True`, `False`  Default value: `False`  | 
| validation\_mini\_batch\_size |  The batch size for validation\. A large `mini_batch_size` usually results in faster training, but it might cause you to run out of memory\. Memory usage is affected by the values of the `mini_batch_size` and `image_shape` parameters, and the backbone architecture\.  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/segmentation-hyperparameters.html) **Optional** Valid values: positive integer Default value: 16  | 
| weight\_decay |  The weight decay coefficient for the `sgd` optimizer\. When you use other optimizers, the algorithm ignores this parameter\.  **Optional** Valid values: 0 < float < 1 Default value: 0\.0001  | 