# Linear Learner Algorithm<a name="linear-learner"></a>

*Linear models* are supervised learning algorithms used for solving either classification or regression problems\. For input, you give the model labeled examples \(*x*, *y*\)\. *x* is a high\-dimensional vector and *y* is a numeric label\. For binary classification problems, the label must be either 0 or 1\. For multiclass classification problems, the labels must be from 0 to `num_classes` \- 1\. For regression problems, *y* is a real number\. The algorithm learns a linear function, or, for classification problems, a linear threshold function, and maps a vector *x* to an approximation of the label *y*\. 

The Amazon SageMaker linear learner algorithm provides a solution for both classification and regression problems\. With the Amazon SageMaker algorithm, you can simultaneously explore different training objectives and choose the best solution from a validation set\. You can also explore a large number of models and choose the best\. The best model optimizes either of the following:
+ Continuous objectives, such as mean square error, cross entropy loss, absolute error\.
+ Discrete objectives suited for classification, such as F1 measure, precision, recall, or accuracy\. 

Compared with methods that provide a solution for only continuous objectives, the Amazon SageMaker linear learner algorithm provides a significant increase in speed over naive hyperparameter optimization techniques\. It is also more convenient\. 

The linear learner algorithm requires a data matrix, with rows representing the observations, and columns representing the dimensions of the features\. It also requires an additional column that contains the labels that match the data points\. At a minimum, Amazon SageMaker linear learner requires you to specify input and output data locations, and objective type \(classification or regression\) as arguments\. The feature dimension is also required\. For more information, see [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)\. You can specify additional parameters in the `HyperParameters` string map of the request body\. These parameters control the optimization procedure, or specifics of the objective function that you train on\. For example, the number of epochs, regularization, and loss type\. 

**Topics**
+ [Input/Output Interface for the Linear Learner Algorithm](#ll-input_output)
+ [EC2 Instance Recommendation for the Linear Learner Algorithm](#ll-instances)
+ [Linear Learner Sample Notebooks](#ll-sample-notebooks)
+ [How Linear Learner Works](ll_how-it-works.md)
+ [Linear Learner Hyperparameters](ll_hyperparameters.md)
+ [Tune a Linear Learner Model](linear-learner-tuning.md)
+ [Linear Learner Response Formats](LL-in-formats.md)

## Input/Output Interface for the Linear Learner Algorithm<a name="ll-input_output"></a>

The Amazon SageMaker linear learner algorithm supports three data channels: train, validation \(optional\), and test \(optional\)\. If you provide validation data, the `S3DataDistributionType` should be `FullyReplicated`\. The algorithm logs validation loss at every epoch, and uses a sample of the validation data to calibrate and select the best model\. If you don't provide validation data, the algorithm uses a sample of the training data to calibrate and select the model\. If you provide test data, the algorithm logs include the test score for the final model\.

**For training**, the linear learner algorithm supports both `recordIO-wrapped protobuf` and `CSV` formats\. For the `application/x-recordio-protobuf` input type, only Float32 tensors are supported\. For the `text/csv` input type, the first column is assumed to be the label, which is the target variable for prediction\. You can use either File mode or Pipe mode to train linear learner models on data that is formatted as `recordIO-wrapped-protobuf` or as `CSV`\.

**For inference**, the linear learner algorithm supports the `application/json`, `application/x-recordio-protobuf`, and `text/csv` formats\. When you make predictions on new data, the format of the response depends on the type of model\. **For regression** \(`predictor_type='regressor'`\), the `score` is the prediction produced by the model\. **For classification** \(`predictor_type='binary_classifier'` or `predictor_type='multiclass_classifier'`\), the model returns a `score` and also a `predicted_label`\. The `predicted_label` is the class predicted by the model and the `score` measures the strength of that prediction\. 
+ **For binary classification**, `predicted_label` is `0` or `1`, and `score` is a single floating point number that indicates how strongly the algorithm believes that the label should be 1\.
+ **For multiclass classification**, the `predicted_class` will be an integer from `0` to `num_classes-1`, and `score` will be a list of one floating point number per class\. 

To interpret the `score` in classification problems, you have to consider the loss function used\. If the `loss` hyperparameter value is `logistic` for binary classification or `softmax_loss` for multiclass classification, then the `score` can be interpreted as the probability of the corresponding class\. These are the loss values used by the linear learner when the `loss` value is `auto` default value\. But if the loss is set to `hinge_loss`, then the score cannot be interpreted as a probability\. This is because hinge loss corresponds to a Support Vector Classifier, which does not produce probability estimates\.

For more information on input and output file formats, see [Linear Learner Response Formats](LL-in-formats.md)\. For more information on inference formats, and the [Linear Learner Sample Notebooks](#ll-sample-notebooks)\.

**To use a model trained with SageMaker Linear Learner in MXNet**
+ Use the following Python code:

  ```
  import mxnet as mx
  import tarfile

  t = tarfile.open('model.tar.gz', 'r:gz')
  t.extractall()
  
  # Load the mxnet module from the model files
  mod = mx.module.Module.load('mx-mod', 0)
  # model's weights
  mod._arg_params['fc0_weight'].asnumpy().flatten()
  
  # model bias
  mod._arg_params['fc0_bias'].asnumpy().flatten()

  mod.bind(data_shapes=data_iter.provide_data) #data_iter is an iterator configured with your test data

  #perform inference
  result = mod.predict(data_iter)
  ```

## EC2 Instance Recommendation for the Linear Learner Algorithm<a name="ll-instances"></a>

You can train the linear learner algorithm on single\- or multi\-machine CPU and GPU instances\. During testing, we have not found substantial evidence that multi\-GPU computers are faster than single\-GPU computers\. Results can vary, depending on your specific use case\.

## Linear Learner Sample Notebooks<a name="ll-sample-notebooks"></a>

For a sample notebook that uses the Amazon SageMaker linear learner algorithm to analyze the images of handwritten digits from zero to nine in the MNIST dataset, see [An Introduction to Linear Learner with MNIST](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/linear_learner_mnist/linear_learner_mnist.ipynb)\. For instructions on how to create and access Jupyter notebook instances that you can use to run the example in Amazon SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all of the Amazon SageMaker samples\. The topic modeling example notebooks using the linear learning algorithm are located in the **Introduction to Amazon algorithms** section\. To open a notebook, choose its **Use** tab and choose **Create copy**\.