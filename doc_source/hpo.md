# Optimizing Hyperparameters<a name="hpo"></a>

You can use Amazon SageMaker Hyperparameter Optimization \(HPO\) to tune a machine learning model by training many variations of the same algorithm with different values for hyperparameters, and choosing the model that perfoms best on your data\.

Before you start using HPO, you should have a well\-defined machine learning problem, including the following:

+ A known dataset

+ The type of algorithm you want to train

+ How you measure success

For example, you might be trying to solve a binary classiﬁcation problem on a marketing dataset where your goal is to maximimize the f1 score of the ﬁnal classiﬁer, by building an [XGBoost Algorithm](xgboost.md) model\.

You should also go through the steps to prepare your dataset and algorithm so that they work in Amazon SageMaker\. You should be able to successfully run a training job at least once before using HPO\. For information about setting up and running a training job, see Train a Model with a Built\-in Algorithm and Deploy It\.


+ [How Amazon SageMaker Hyperparameter Optimization Works](#hpo-how-it-works)
+ [Setting up Hyperparameter Optimization](hpo-setup.md)
+ [Example: HyperParameter Optimization Job](hpo-ex.md)
+ [Design Considerations](hpo-considerations.md)
+ [Limitations of Amazon SageMaker Hyperparameter Optimization](hpo-limitations.md)
+ [Troubleshooting Amazon SageMaker Hyperparameter Optimization](hpo-troubleshooting.md)

## How Amazon SageMaker Hyperparameter Optimization Works<a name="hpo-how-it-works"></a>

Amazon SageMaker HPO finds the best version of a model by running mamny training jobs on your dataset using the altorithm and ranges of hyperparameters that you specify, and choosing the values for the hyperparameters that result in the best evaluation metric\. Hyperparameters are things like the learning rate, regularization, and number of layers in a neural network\.

HPO is a supervised learning regression problem: given a set of input features \(the hyperparameters\), try to predict the quality metric of your choosing, whether it is validation accuracy, f1 score on test set, runtime speed, or any other metric your algorithm defines\. To solve this problem, HPO makes guesses about which hyperparameter combinations are likely to get the best results, and runs training jobs to test these guesses\.

Tthe techniques that HPO use are referred to as Bayesian Optimization\. They rely on Gaussian Process models to predict the accuracy metric for the specified inputs\. Gaussian Process models tend to perform very well with small data sets, but are extremely computationally intensive relative to other techniques\. This is ﬁne given that the training jobs they are working with are even more computationally intensive\.

Further reading:

+ [A Tutorial on Bayesian Optimization of Expensive Cost Functions, with Application to Active User Modeling and Hierarchical Reinforcement Learning](https://arxiv.org/abs/1012.2599)

+ [Practical Bayesian Optimization of Machine Learning Algorithms](https://arxiv.org/abs/1206.2944)

+ [Taking the Human Out of the Loop: A Review of Bayesian Optimization](http://ieeexplore.ieee.org/document/7352306/?reload=true)