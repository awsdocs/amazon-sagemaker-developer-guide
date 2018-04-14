# How It Works<a name="ll_how-it-works"></a>

**Note**  
For best results ensure your data is shuffled before training\. Training with unshuffled data may cause training to fail\. 

## Step 1: Preprocessing<a name="step1-preprocessing"></a>

Normalization, or feature scaling, is an important preprocessing step for certain loss functions to ensure the resulting model does not become dominated by the weight of a single feature\. The SageMaker Linear Learner algorithm has a normalization option to assist with this preprocessing step\. If normalization is turned on the algorithm first goes over a small sample of the data to learn the mean value and standard deviation for each feature and the label\. The data is then shifted to have mean zero and scaled to have unit standard deviation\. 

Normalization is enabled by default for both features and labels\. If enabled for binary classification only the features are normalized\.

## Step 2: Training<a name="step2-training"></a>

Training is done via a distributed implementation of stochastic gradient descent\. Options for training include the loss function to optimize such as squared loss, huber loss, absolute loss, etc, and hyperparameters such as momentum, learning rate, and L1/L2 regularization terms\. 

If you are unsure of how to tune your hyperparameters when starting out try using the default values but setting the **num_models** option to an integer value between 1 and 32\. This option allows the SageMaker Linear Learner algorithm to train up to the specified number of models in parallel using different values of hyperparameters with each to try to find the most optimal model\.

## Step 3: Validation and Setting the Threshold<a name="step3-validation"></a>

When training multiple models in parallel the models are evaluated against a validation set to select the most optimal model once training is complete\. For regression the most optimal model is the one that achieves the best loss on the validation set\. For classification a sample of the validation set is used to calibrate the classification threshold and the most optimal model is selected as the one that achieves the best binary classification selection criteria on the validation set, such as the F1 measure, accuracy, cross-entropy loss, and so on\. 

**Note**  
If the algorithm is not provided a validation set is not provided then selecting the evaluating and selecting the most optimal model is not possible\. To take advantage of parallel training and model selection ensure you provide a validation set to the algorithm\. 