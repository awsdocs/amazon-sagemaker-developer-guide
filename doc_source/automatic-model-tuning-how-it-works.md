# How Hyperparameter Tuning Works<a name="automatic-model-tuning-how-it-works"></a>

When you build complex machine learning systems like deep learning neural networks, exploring all of the possible combinations is impractical\. Hyperparameter tuning can accelerate your productivity by trying many variations of a model\. It looks for the best model automatically by focusing on the most promising combinations of hyperparameter values within the ranges that you specify\. To get good results, you need to choose the right ranges to explore\. 

Use the [API reference guide](https://docs.aws.amazon.com/sagemaker/latest/APIReference/Welcome.html?icmpid=docs_sagemaker_lp) to understand how to interact with hyperparameter tuning\. The examples on this page can be found in the [HyperParameterTuningJobConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HyperParameterTuningJobConfig.html) and [HyperbandStrategyConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_HyperbandStrategyConfig.html) APIs\.

**Note**  
Because the algorithm itself is stochastic, it’s possible that the hyperparameter tuning model will fail to converge on the best answer, even if the best possible combination of values is within the ranges that you choose\.

## Grid Search<a name="automatic-tuning-grid-search"></a>

 When using grid search, hyperparameter tuning chooses combinations of values from the range of categorical values that you specify when you create the job\. Only categorical parameters are supported when using the grid search strategy\. You do not need to specify the `MaxNumberOfTrainingJobs`\. The number of training jobs created by the tuning job will be automatically calculated to be the total number of distinct categorical combinations possible\. If specified, the value of `MaxNumberOfTrainingJobs` should equal the total number of distinct categorical combinations possible\.

## Random Search<a name="automatic-tuning-random-search"></a>

When using random search, hyperparameter tuning chooses a random combination of values from within the ranges that you specify for hyperparameters for each training job it launches\. Because the choice of hyperparameter values doesn't depend on the results of previous training jobs, you can run the maximum number of concurrent training jobs without affecting the performance of the tuning\.

For an example notebook that uses random search, see [https://github\.com/awslabs/amazon\-sagemaker\-examples/blob/master/hyperparameter\_tuning/xgboost\_random\_log/hpo\_xgboost\_random\_log\.ipynb](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/hyperparameter_tuning/xgboost_random_log/hpo_xgboost_random_log.ipynb)\.

## Bayesian Optimization<a name="automatic-tuning-bayesian-optimization.title"></a>

Bayesian optimization treats hyperparameter tuning like a *[regression](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html#[regression])* problem\. Given a set of input features \(the hyperparameters\), hyperparameter tuning optimizes a model for the metric that you choose\. To solve a regression problem, hyperparameter tuning makes guesses about which hyperparameter combinations are likely to get the best results, and runs training jobs to test these values\. After testing a set of hyperparameter values, hyperparameter tuning uses regression to choose the next set of hyperparameter values to test\.

Hyperparameter tuning uses an Amazon SageMaker implementation of Bayesian optimization\.

When choosing the best hyperparameters for the next training job, hyperparameter tuning considers everything that it knows about this problem so far\. Sometimes it chooses a combination of hyperparameter values close to the combination that resulted in the best previous training job to incrementally improve performance\. This allows hyperparameter tuning to exploit the best known results\. Other times, it chooses a set of hyperparameter values far removed from those it has tried\. This allows it to explore the range of hyperparameter values to try to find new areas that are not yet well understood\. The explore/exploit trade\-off is common in many machine learning problems\.

For more information about Bayesian optimization, see the following:

**Basic Topics on Bayesian Optimization**
+ [A Tutorial on Bayesian Optimization of Expensive Cost Functions, with Application to Active User Modeling and Hierarchical Reinforcement Learning](https://arxiv.org/abs/1012.2599)
+ [Practical Bayesian Optimization of Machine Learning Algorithms](https://arxiv.org/abs/1206.2944)
+ [Taking the Human Out of the Loop: A Review of Bayesian Optimization](http://ieeexplore.ieee.org/document/7352306/?reload=true)

**Speeding up Bayesian Optimization**
+ [Google Vizier: A Service for Black\-Box Optimization](https://dl.acm.org/citation.cfm?id=3098043)
+ [Learning Curve Prediction with Bayesian Neural Networks](https://openreview.net/forum?id=S11KBYclx)
+ [Speeding up automatic hyperparameter optimization of deep neural networks by extrapolation of learning curves](https://dl.acm.org/citation.cfm?id=2832731)

**Advanced Modeling and Transfer Learning**
+ [Scalable Hyperparameter Transfer Learning](https://papers.nips.cc/paper/7917-scalable-hyperparameter-transfer-learning)
+ [Bayesian Optimization with Tree\-structured Dependencies](http://proceedings.mlr.press/v70/jenatton17a.html)
+ [Bayesian Optimization with Robust Bayesian Neural Networks](https://papers.nips.cc/paper/6116-bayesian-optimization-with-robust-bayesian-neural-networks)
+ [Scalable Bayesian Optimization Using Deep Neural Networks](http://proceedings.mlr.press/v37/snoek15.pdf)
+ [Input Warping for Bayesian Optimization of Non\-stationary Functions]( https://arxiv.org/abs/1402.0929)

## Hyperband<a name="automatic-tuning-hyperband"></a>

Hyperband is a multi\-fidelity based tuning strategy that dynamically reallocates resources\. Hyperband uses both intermediate and final results of training jobs to re\-allocate epochs to well\-utilized hyperparameter configurations and automatically stops those that underperform\. It also seamlessly scales to using many parallel training jobs\. These features can significantly speed up hyperparameter tuning over random search and Bayesian optimization strategies\.

Hyperband should only be used to tune iterative algorithms that publish results at different resource levels\. For example, Hyperband can be used to tune a neural network for image classification which publishes accuracy metrics after every epoch\.

For more information about Hyperband, see the following links:
+ [Hyperband: A Novel Bandit\-Based Approach to Hyperparameter Optimization](https://arxiv.org/pdf/1603.06560.pdf)
+ [Massively Parallel Hyperparameter Tuning](https://liamcli.com/assets/pdf/asha_arxiv.pdf)
+ [BOHB: Robust and Efficient Hyperparameter Optimization at Scale](http://proceedings.mlr.press/v80/falkner18a/falkner18a.pdf)
+ [Model\-based Asynchronous Hyperparameter and Neural Architecture Search](https://openreview.net/pdf?id=a2rFihIU7i)

### Hyperband with early stopping<a name="automatic-tuning-hyperband-early-stopping"></a>

Training jobs can be stopped early when they are unlikely to improve the objective metric of the hyperparameter tuning job\. This can help both reduce compute time and avoid overfitting your model\. Hyperband uses an advanced internal mechanism to apply early stopping\. Thus, the parameter `TrainingJobEarlyStoppingType` in the `HyperParameterTuningJobConfig` API must be set to `OFF` when using Hyperband's internal early stopping feature\.

**Note**  
Hyperparameter tuning might not improve your model\. It is an advanced tool for building machine solutions, and, as such, should be considered part of the scientific development process\. 