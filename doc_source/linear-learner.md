# Linear Learner<a name="linear-learner"></a>

Linear models are supervised learning algorithms used for solving either classification or regression problems\. As input the model is given labeled examples \(**x**, **y**\)\. **x** is a high dimensional vector and **y** is a numeric label\. For \(binary\) classification problems, the algorithm expects the label to be either 0 or 1\. For regression problems, **y** is a real number\. The algorithm learns a linear function, or linear threshold function for classification, mapping a vector **x** to an approximation of the label **y**\. 

The Amazon SageMaker linear learner algorithm provides a solution for both classification and regression problems\. This allows you to simultaneously explore different training objectives and choose the best solution from a validation set\. It also allows you to explore a large number of models and choose the best, which optimizes either continuous objectives—such as mean square error, cross entropy loss, absolute error, and so on—or discrete objectives suited for classification, such as F1 measure, precision@recall, or accuracy\. When compared with solutions providing a solution to only continuous objectives, the implementation provides a significant increase in speed over naive hyperparameter optimization techniques and added convenience\. 

The linear learner expects a data matrix, with rows representing the observations, and columns the dimensions of the features\. It also requires an additional column containing the labels that match the data points\. At a minimum, Amazon SageMaker linear learner requires you to specify input and output data locations, and objective type \(classification or regression\) as arguments\. The feature dimension is also required\. For more information, see [CreateTrainingJob](API_CreateTrainingJob.md)\. You can specify additional parameters in the HyperParameters string map of the request body\. These parameters control the optimization procedure, or specifics of the objective function on which you train\. Examples include the number of epochs, regularization, and loss type\. 

## Input/Output Interface<a name="ll-input_output"></a>

Amazon SageMaker linear learner supports three data channels: train, validation, and test\. The validation data channel is optional\. If you provide validation data, it should be `FullyReplicated`\. The validation loss is logged at every epoch, and a sample of the validation data is used to calibrate and select the best model\. If you don't provide validation data, the final model calibration and selection uses a sample of the training data\. The test data channel is also optional\. If test data is provided, the algorithm logs contain the test score for the final model\.

Linear learner supports both `recordIO wrapped protobuf` and `CSV`\. For input type `x-recordio-protobuf`, only Float32 tensors are supported\. For input type `text/csv`, the first column is assumed to be the label, which is the target variable for prediction\. Linear learner can be trained in File or Pipe mode when using recordIO\-wrapped protobuf, but only in File mode for the `CSV` format\.

For inference, Linear Learner supports the `application/json`, `x-recordio-protobuf`, and `text/csv` formats\. For binary classification models, both the score and the predicted label are returned\. For regression, just the score is returned\.

For more details on training and inference file formats, see example notebooks\.

## EC2 Instance Recommendation<a name="ll-instances"></a>

Linear learner can be trained on single\- or multi\-machine CPU and GPU instances\. During our testing, we have not found substantial evidence to multi\-GPU to be faster than single GPU, but results vary depending on the specific use case\.