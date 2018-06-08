# How It Works<a name="ll_how-it-works"></a>

**Note**  
We assume that the input data is shuffled\. If not, for example if the data is ordered by label, the method fails\. 

## Step 1: Preprocessing<a name="step1-preprocessing"></a>

If the option is turned on, the algorithm first goes over a small sample of the data to learn its characteristics\. For every feature and for the label, you learn the mean value and the standard deviation\. 

This information is used during training\. Based on the configuration, you normalize the data\. That is, you shift it to have mean zero and scale it to have unit standard deviation\. When the *auto* \(default\) value is specified to decide the normalization you: 
+ Shift and scale the label for regression problems, and leave it as is for classification problems
+ Always scale the features
+ Shift the features only for dense data

## Step 2: Training<a name="step2-training"></a>

You train using a distributed implementation of stochastic gradient descent\. The input allows you to control specifics of the optimization procedure by choosing the exact optimization algorithm, for example, Adam, Adagrad, SGD, and so on, and their parameters, such as momentum, learning rate, and the learning rate schedule\. Without specified details, choose a default option that works for the majority of datasets\. 

During training, you simultaneously optimize multiple models, each with slightly different objectives: in other words, vary L1 or L2 regularization and try out different optimizer settings\. 

## Step 3: Validation and Setting the Threshold<a name="step3-validation"></a>

When the training is done, evaluate the different models on a validation set\. For regression problems, output the model obtaining the best score on the validation set\. When the objective is classification, use a sample of \(raw prediction, label\) pairs to tune the threshold for a provided objective\. The raw prediction is the output of the trained linear function\. Allow classification objectives based on the predicted label, such as F1 measure, accuracy, precision@recall, and so on\. Choose the model that achieves the best score on the validation set\. 

**Note**  
If you don't provide a validation set, the algorithm optimizes over the training set\. In such a scenario, avoid exploring different regularization procedures\. 