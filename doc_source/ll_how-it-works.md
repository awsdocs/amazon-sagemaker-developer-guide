# How Linear Learner Works<a name="ll_how-it-works"></a>

There are three steps involved in the implementation of the linear learner algorithm: preprocess, train, and validate\. 

## Step 1: Preprocess<a name="step1-preprocessing"></a>

If the option is turned on, the algorithm first goes over a small sample of the data to learn its characteristics\. The algorithm learns the mean value and the standard deviation for every feature and label\. It uses this information during training\. 

**Note**  
Input data for the linear learner algorithm must be shuffled\. If it isn't—for example, if the data is ordered by label—the algorithm fails\. 

You can configure the algorithm to normalize the data with the \(`normalize_label`\) hyperparameter\. Label normalizationshifts the label to have a mean of zero and scales it to have unit standard deviation\. If you want the algorithm to normalize the data for you, specify the `auto` \(default\) value for the `normalize_data` hyperparameter\. When auto normalization is enabled, the algorithm does the following: 
+ For regression problems, shifts and scales the label\. For classification problems, leaves the label as is\.
+ Always scales the features\.
+ Shifts the features only for dense data\.

## Step 2: Train<a name="step2-training"></a>

With the linear learner algorithm, you train with a distributed implementation of stochastic gradient descent \(SGD\)\. You can control the optimization process by choosing the optimization algorithm\. For example, you can choose to use Adam, AdaGrad, stochastic gradient descent, or other optimization algorithms\. You also specify their hyperparameters, such as momentum, learning rate, and the learning rate schedule\. If you aren't sure which algorithm or hyperparameter value to use, choose a default that works for the majority of datasets\. 

During training, you simultaneously optimize multiple models, each with slightly different objectives\. For example, you vary L1 or L2 regularization and try out different optimizer settings\. 

## Step 3: Validate and Set the Threshold<a name="step3-validation"></a>

When training is done, you can evaluate the models on a validation set\. For regression problems, output the model that gets the best score on the validation set\. For classification problems, use a sample of \(raw prediction, label\) pairs to tune the threshold for a provided objective\. The raw prediction is the output of the trained linear function\. Allow classification objectives based on the predicted label, such as F1 measure, accuracy, precision@recall, and so on\. Choose the model that achieves the best score on the validation set\. 

**Note**  
If you don't provide a validation set, the algorithm optimizes over the training set\. In that case, you do not need to explore other regularization procedures\. 