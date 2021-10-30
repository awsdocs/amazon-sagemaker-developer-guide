# Model support, metrics, and validation<a name="autopilot-model-support-validation"></a>

Amazon SageMaker Autopilot supports three types of machine learning algorithms to address machine learning problems, reports on various quality and objective metrics, and uses cross\-validation automatically when needed\.

**Topics**
+ [Autopilot algorithm support](#autopilot-algorithm-suppprt)
+ [Autopilot candidate metrics](#autopilot-metrics)
+ [Autopilot cross\-validation](#autopilot-cross-validation)

## Autopilot algorithm support<a name="autopilot-algorithm-suppprt"></a>

The three types of machine learning algorithms supported by Autopilot are:
+  [Linear Learner Algorithm](linear-learner.md): a supervised learning algorithms used for solving either classification or regression problems\.
+ [XGBoost Algorithm](xgboost.md): a supervised learning algorithm that attempts to accurately predict a target variable by combining an ensemble of estimates from a set of simpler and weaker models\.
+ Deep Learning Algorithm: a multilayer perceptron \(MLP\), a feedforward artificial neural network that can handle data that is not linearly separable\.

**Note**  
You do not need to specify an algorithm to use for your machine learning problem\. Autopilot automatically selects the appropriate algorithm to train\. 

## Autopilot candidate metrics<a name="autopilot-metrics"></a>

Amazon SageMaker Autopilot produces metrics used to measure the predictive quality of machine learning model candidates\. The metrics calculated for candidates are specified using an array of [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_MetricDatum.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_MetricDatum.html) types\. The following list contains the names of the metrics currently available\.
+ `MSE`: The mean squared error \(MSE\) is the average of the squared differences between the predicted and actual values\. It is used for regression\. MSE values are always positive: the better a model is at predicting the actual values, the smaller the MSE value is\.
+ `Accuracy`: The ratio of the number of correctly classified items to the total number of \(correctly and incorrectly\) classified items\. It is used for binary and multiclass classification\. It measures how close the predicted class values are to the actual values\. Accuracy values vary between zero and one: one indicates perfect accuracy and zero indicates perfect inaccuracy\.
+ `F1`: The F1 score is the harmonic mean of the precision and recall\. It is used for binary classification into classes traditionally referred to as positive and negative\. Predictions are said to be true when they match their actual \(correct\) class and false when they do not\. Precision is the ratio of the true positive predictions to all positive predictions \(including the false positives\) in a dataset and measures the quality of the prediction when it predicts the positive class\. Recall \(or sensitivity\) is the ratio of the true positive predictions to all actual positive instances and measures how completely a model predicts the actual class members in a dataset\. The standard F1 score weighs precision and recall equally\. But which metric is paramount typically depends on specific aspects of a problem\. F1 scores vary between zero and one: one indicates the best possible performance and zero the worst\.
+ `AUC`: The area under the curve \(AUC\) metric is used to compare and evaluate binary classification by algorithms such as logistic regression that return probabilities\. A threshold is needed to map the probabilities into classifications\. The relevant curve is the receiver operating characteristic curve that plots the true positive rate \(TPR\) of predictions \(or recall\) against the false positive rate \(FPR\) as a function of the threshold value, above which a prediction is considered positive\. Increasing the threshold results in fewer false positives but more false negatives\. AUC is the area under this receiver operating characteristic curve and so provides an aggregated measure of the model performance across all possible classification thresholds\. AUC scores vary between zero and one: a score of one indicates perfect accuracy and a score of one half indicates that the prediction is not better than a random classifier\. 
+ `F1macro`: The F1macro score applies F1 scoring to multiclass classification\. In this context, you have multiple classes to predict\. You just calculate the precision and recall for each class as you did for the positive class in binary classification\. Then, use these values to calculate the F1 score for each class and average them to obtain the F1macro score\. F1macro scores vary between zero and one: one indicates the best possible performance and zero the worst\.

The metrics automatically calculated for a candidate model are determined by the type of problem being addressed\.
+ regression: `MSE`
+ binary classification: `Accuracy`, `F1`, `AUC`
+ multiclass classification: `Accuracy`, `F1macro`

## Autopilot cross\-validation<a name="autopilot-cross-validation"></a>

Autopilot uses the k\-fold cross\-validation method automatically when needed\. You can use this method to assess how well a model trained on a dataset can predict the values of an unseen validation dataset drawn from the same population\. This method is especially important, for example, when training on datasets that have a limited number of training instances\. It can protect against problems like overfitting and selection bias that can prevent a model from being more generally applicable to the population sampled\.

For example, the Boston Housing dataset contains only 861 samples\. If you try to build a model to predict house sale prices using this dataset without cross\-validation, you risk training on a dataset that is not representative of the Boston housing stock\. Typically, you would split the data only once into training and validation subsets\. If the training fold happened to contain data mainly from suburbs that were not representative of the rest of the city, you would likely overfit on this biased selection\. Cross\-validation reduces the risk of these errors by making full and randomized use of the available data for training and validation\.

K\-fold cross\-validation randomly splits a training dataset into *k* equally sized subsamples or folds\. Then models are trained on *k\-1* folds and tested against the remaining fold, which is retained as a validation dataset\. The process is repeated *k* times, using each fold once as the validation dataset\. Autopilot applies the cross\-validation method to datasets with 50,000 or fewer training instances\. It uses a *k* value of 5 on the candidate algorithms used to model the dataset\. Multiple models are trained on different splits and the models are stored separately\. When the training procedure is finished, the validation metrics for each of the models are averaged to produce a single estimation metric\. When cross\-validation is applied by Autopilot to smaller datasets, the training time increases 20% on average\. If your dataset is complicated, the training time may increase more significantly\. For predictions, Autopilot uses the ensemble of cross\-validation models from the trial with the best validation metric\.

You can see the training and validation metrics from each fold in your `/aws/sagemaker/TrainingJobs` CloudWatch Logs\. For more information about CloudWatch Logs, see [Log Amazon SageMaker Events with Amazon CloudWatch](logging-cloudwatch.md)\. The validation metric for the models trained by Autopilot is presented as the objective metric in model leader board\. Autopilot uses the default validation metric for each problem type it handles unless you specify otherwise\. For more information on the metrics Autopilot uses, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobObjective.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobObjective.html)\. You can deploy Autopilot models built using cross\-validation just as you would any other Autopilot or SageMaker model\.