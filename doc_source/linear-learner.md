# Linear learner algorithm<a name="linear-learner"></a>

*Linear models* are supervised learning algorithms used for solving either classification or regression problems\. For input, you give the model labeled examples \(*x*, *y*\)\. *x* is a high\-dimensional vector and *y* is a numeric label\. For binary classification problems, the label must be either 0 or 1\. For multiclass classification problems, the labels must be from 0 to `num_classes` \- 1\. For regression problems, *y* is a real number\. The algorithm learns a linear function, or, for classification problems, a linear threshold function, and maps a vector *x* to an approximation of the label *y*\. 

The Amazon SageMaker linear learner algorithm provides a solution for both classification and regression problems\. With the SageMaker algorithm, you can simultaneously explore different training objectives and choose the best solution from a validation set\. You can also explore a large number of models and choose the best\. The best model optimizes either of the following:
+ Continuous objectives, such as mean square error, cross entropy loss, absolute error\.
+ Discrete objectives suited for classification, such as F1 measure, precision, recall, or accuracy\. 

Compared with methods that provide a solution for only continuous objectives, the SageMaker linear learner algorithm provides a significant increase in speed over naive hyperparameter optimization techniques\. It is also more convenient\. 

The linear learner algorithm requires a data matrix, with rows representing the observations, and columns representing the dimensions of the features\. It also requires an additional column that contains the labels that match the data points\. At a minimum, Amazon SageMaker linear learner requires you to specify input and output data locations, and objective type \(classification or regression\) as arguments\. The feature dimension is also required\. For more information, see [ `CreateTrainingJob`](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateTrainingJob.html)\. You can specify additional parameters in the `HyperParameters` string map of the request body\. These parameters control the optimization procedure, or specifics of the objective function that you train on\. For example, the number of epochs, regularization, and loss type\. 

**Topics**
+ [Input/Output interface for the linear learner algorithm](#ll-input_output)
+ [EC2 instance recommendation for the linear learner algorithm](#ll-instances)
+ [Linear learner sample notebooks](#ll-sample-notebooks)
+ [How linear learner works](ll_how-it-works.md)
+ [Linear learner hyperparameters](ll_hyperparameters.md)
+ [Tune a linear learner model](linear-learner-tuning.md)
+ [Linear learner response formats](LL-in-formats.md)

## Input/Output interface for the linear learner algorithm<a name="ll-input_output"></a>

The Amazon SageMaker linear learner algorithm supports three data channels: train, validation \(optional\), and test \(optional\)\. If you provide validation data, the `S3DataDistributionType` should be `FullyReplicated`\. The algorithm logs validation loss at every epoch, and uses a sample of the validation data to calibrate and select the best model\. If you don't provide validation data, the algorithm uses a sample of the training data to calibrate and select the model\. If you provide test data, the algorithm logs include the test score for the final model\.

**For training**, the linear learner algorithm supports both `recordIO-wrapped protobuf` and `CSV` formats\. For the `application/x-recordio-protobuf` input type, only Float32 tensors are supported\. For the `text/csv` input type, the first column is assumed to be the label, which is the target variable for prediction\. You can use either File mode or Pipe mode to train linear learner models on data that is formatted as `recordIO-wrapped-protobuf` or as `CSV`\.

**For inference**, the linear learner algorithm supports the `application/json`, `application/x-recordio-protobuf`, and `text/csv` formats\. When you make predictions on new data, the format of the response depends on the type of model\. **For regression** \(`predictor_type='regressor'`\), the `score` is the prediction produced by the model\. **For classification** \(`predictor_type='binary_classifier'` or `predictor_type='multiclass_classifier'`\), the model returns a `score` and also a `predicted_label`\. The `predicted_label` is the class predicted by the model and the `score` measures the strength of that prediction\. 
+ **For binary classification**, `predicted_label` is `0` or `1`, and `score` is a single floating point number that indicates how strongly the algorithm believes that the label should be 1\.
+ **For multiclass classification**, the `predicted_class` will be an integer from `0` to `num_classes-1`, and `score` will be a list of one floating point number per class\. 

To interpret the `score` in classification problems, you have to consider the loss function used\. If the `loss` hyperparameter value is `logistic` for binary classification or `softmax_loss` for multiclass classification, then the `score` can be interpreted as the probability of the corresponding class\. These are the loss values used by the linear learner when the `loss` value is `auto` default value\. But if the loss is set to `hinge_loss`, then the score cannot be interpreted as a probability\. This is because hinge loss corresponds to a Support Vector Classifier, which does not produce probability estimates\.

For more information on input and output file formats, see [Linear learner response formats](LL-in-formats.md)\. For more information on inference formats, and the [Linear learner sample notebooks](#ll-sample-notebooks)\.

## EC2 instance recommendation for the linear learner algorithm<a name="ll-instances"></a>

You can train the linear learner algorithm on single\- or multi\-machine CPU and GPU instances\. During testing, we have not found substantial evidence that multi\-GPU computers are faster than single\-GPU computers\. Results can vary, depending on your specific use case\.

## Linear learner sample notebooks<a name="ll-sample-notebooks"></a>

 The following table outlines a variety of sample notebooks that address different use cases of Amazon SageMaker linear learner algorithm\. 


****  

| **Title** | **Description** | 
| --- | --- | 
|  [An Introduction with the MNIST dataset](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/linear_learner_mnist/linear_learner_mnist.ipynb)  |   Using the MNIST dataset, we train a binary classifier to predict a single digit\.  | 
|  [Predict Breast Cancer](https://github.com/awslabs/amazon-sagemaker-examples/tree/master/introduction_to_applying_machine_learning/breast_cancer_prediction)  |   Using UCI's Breast Cancer dataset, we train a model to predict Breast Cancer\.   | 
|  [How to Build a Multiclass Classifier?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/scientific_details_of_algorithms/linear_learner_multiclass_classification/linear_learner_multiclass_classification.ipynb)  |   Using UCI's Covertype dataset, we demonstrate how to train a multiclass classifier\.   | 
|  [How to Build a Machine Learning \(ML\) Pipeline for Inference? ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-python-sdk/scikit_learn_inference_pipeline/Inference%20Pipeline%20with%20Scikit-learn%20and%20Linear%20Learner.ipynb)  |   Using a Scikit\-learn container, we demonstrate how to build an end\-to\-end ML pipeline\.   | 

 For instructions on how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all of the SageMaker samples\. The topic modeling example notebooks using the linear learning algorithm are located in the **Introduction to Amazon algorithms** section\. To open a notebook, choose its **Use** tab and choose **Create copy**\. 