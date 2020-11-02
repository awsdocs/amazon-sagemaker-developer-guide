# How linear learner works<a name="ll_how-it-works"></a>

There are three steps involved in the implementation of the linear learner algorithm: preprocess, train, and validate\. Step 4 provides example code that shows how to deploy a trained model\.

## Step 1: Preprocess<a name="step1-preprocessing"></a>

Normalization, or feature scaling, is an important preprocessing step for certain loss functions that ensures the model being trained on a dataset does not become dominated by the weight of a single feature\. The Amazon SageMaker Linear Learner algorithm has a normalization option to assist with this preprocessing step\. If normalization is turned on, the algorithm first goes over a small sample of the data to learn the mean value and standard deviation for each feature and for the label\. Each of the features in the full dataset is then shifted to have mean of zero and scaled to have a unit standard deviation\.

**Note**  
For best results, ensure your data is shuffled before training\. Training with unshuffled data may cause training to fail\. 

You can configure whether the linear learner algorithm normalizes the feature data and the labels using the `normalize_data` and `normalize_label` hyperparameters, respectively\. Normalization is enabled by default for both features and labels for regression\. Only the features can be normalized for binary classification and this is the default behavior\. 

## Step 2: Train<a name="step2-training"></a>

With the linear learner algorithm, you train with a distributed implementation of stochastic gradient descent \(SGD\)\. You can control the optimization process by choosing the optimization algorithm\. For example, you can choose to use Adam, AdaGrad, stochastic gradient descent, or other optimization algorithms\. You also specify their hyperparameters, such as momentum, learning rate, and the learning rate schedule\. If you aren't sure which algorithm or hyperparameter value to use, choose a default that works for the majority of datasets\. 

During training, you simultaneously optimize multiple models, each with slightly different objectives\. For example, you vary L1 or L2 regularization and try out different optimizer settings\. 

## Step 3: Validate and set the threshold<a name="step3-validation"></a>

When training multiple models in parallel, the models are evaluated against a validation set to select the most optimal model once training is complete\. For regression, the most optimal model is the one that achieves the best loss on the validation set\. For classification, a sample of the validation set is used to calibrate the classification threshold\. The most optimal model selected is the one that achieves the best binary classification selection criteria on the validation set\. Examples of such criteria include the F1 measure, accuracy, and cross\-entropy loss\. 

**Note**  
If the algorithm is not provided a validation set, then evaluating and selecting the most optimal model is not possible\. To take advantage of parallel training and model selection ensure you provide a validation set to the algorithm\. 

## Step 4: Deploy a trained linear model<a name="step4-deploy-trained-ll-model"></a>

Here is an example of Python code that you can use to deploy a model in MXNet that has been trained with SageMaker linear learner 

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