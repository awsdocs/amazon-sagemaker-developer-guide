# Model support and validation<a name="autopilot-model-support-validation"></a>

Amazon SageMaker Autopilot supports three types of machine learning algorithms to address machine learning problems and uses cross\-validation automtically when needed\.

**Topics**
+ [Autopilot algorithm support](#autopilot-algorithm-suppprt)
+ [Autopilot cross\-validation](#autopilot-cross-validation)

## Autopilot algorithm support<a name="autopilot-algorithm-suppprt"></a>

The three types of machine learning algorithms supported by Autopilot are:
+  [Linear Learner Algorithm](linear-learner.md): a supervised learning algorithms used for solving either classification or regression problems\.
+ [XGBoost Algorithm](xgboost.md): a supervised learning algorithm that attempts to accurately predict a target variable by combining an ensemble of estimates from a set of simpler and weaker models\.
+ Deep Learning Algorithm: multilayer perceptron \(MLP\), a feedforward artificial neural network that can handle data that is not linear separable\.

**Note**  
You do not need to specify an algorithm to use for your machine learning problem\. Autopilot automatically selects the appropriate algorithm to train\. 

## Autopilot cross\-validation<a name="autopilot-cross-validation"></a>

Autopilot uses the k\-fold cross\-validation method automatically when needed\. You can use this method to assess how well a model trained on a dataset can predict the values of an unseen validation dataset drawn from the same population\. This method is especially important, for example, when training on datasets that have a limited number of training instances\. It can protect against problems like overfitting and selection bias that can prevent a model from being more generally applicable to the population sampled\.

For example, the Boston Housing dataset contains only 861 samples\. If you try to build a model to predict house sale prices using this dataset without cross\-validation, you risk training on a data set that is not representative of the Boston housing stock\. Typically, you would split the data only once into training and validation subsets\. If the training fold happened to contain data mainly from suburbs that were not representative of the rest of the city, you would likely overfit on this biased selection\. Cross\-validation reduces the risk of these errors by making full and randomized use of the available data for training and validation\.

K\-fold cross\-validation randomly splits a training dataset into *k* equally sized subsamples or folds\. Then models are trained on *k\-1* folds and tested against the remaining fold, which is retained as a validation dataset\. The process is repeated *k* times, using each fold once as the validation dataset\. Autopilot applies the cross\-validation method to datasets with 50,000 or fewer training instances\. It uses a *k* value of 5 on the candidate algorithms used to model the dataset\. Multiple models are trained on different splits and the models are stored separately\. When the training procedure is finished, the validation metrics for each of the models are averaged to produce a single estimation metric\. For predictions, Autopilot uses the ensemble of cross\-validation models from the trial with the best validation metric\.

You can see the training and validation metrics from each fold in your `/aws/sagemaker/TrainingJobs` CloudWatch Logs\. For more information about CloudWatch Logs, see [Log Amazon SageMaker Events with Amazon CloudWatch](logging-cloudwatch.md)\. The validation metric for the models trained by Autopilot is presented as the objective metric in model leader board\. Autopilot uses the default validation metric for each problem type it handles unless you specify otherwise\. For more information on the metrics Autopilot uses, see [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobObjective.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_AutoMLJobObjective.html)\. You can deploy Autopilot models built using cross\-validation just as you would any other Autopilot or SageMaker model\.
