# How Hyperparameter Tuning Works<a name="automatic-model-tuning-how-it-works"></a>

Hyperparameter tuning is a supervised machine learning regression problem\. Given a set of input features \(the hyperparameters\), hyperparameter tuning optimizes a model for the metric that you choose\. You can choose any metric that the algorithm you use defines\. To solve a regression problem, hyperparameter tuning makes guesses about which hyperparameter combinations are likely to get the best results, and runs training jobs to test these guesses\. After testing the first set of hyperparameter values, hyperparameter tuning uses regression to choose the next set of hyperparameter values to test\.

Hyperparameter tuning uses an Amazon SageMaker implementation of Bayesian optimization\.

When choosing the best hyperparameters for the next training job, hyperparameter tuning considers everything it knows about this problem so far\. Sometimes it chooses a point that is likely to produce an incremental improvement on the best result found so far\. This allows hyperparameter tuning to exploit the best known results\. Other times it chooses a set of hyperparameters far removed from those it has tried\. This allows it to explore the space to try to find new areas that are not well understood\. The explore/exploit trade\-off is common in many machine learning problems\.

For more information about Bayesian optimization, see the following:
+ [A Tutorial on Bayesian Optimization of Expensive Cost Functions, with Application to Active User Modeling and Hierarchical Reinforcement Learning](https://arxiv.org/abs/1012.2599)
+ [Practical Bayesian Optimization of Machine Learning Algorithms](https://arxiv.org/abs/1206.2944)
+ [Taking the Human Out of the Loop: A Review of Bayesian Optimization](http://ieeexplore.ieee.org/document/7352306/?reload=true)

**Note**  
Hyperparameter tuning might not improve your model\. It is an advanced tool for building machine solutions, and, as such, should be considered part of the scientific development process\.   
When you build complex machine learning systems like deep learning neural networks, exploring all of the possible combinations is impractical\. Hyperparameter tuning can accelerate your productivity by trying many variations of a model, focusing on the most promising combinations of hyperparameter values within the ranges that you specify\. To get good results, you need to choose the right ranges to explore\. Because the algorithm itself is stochastic, it’s possible that the hyperparameter tuning model will fail to converge on the best answer, even if the ranges specified are correct\. 