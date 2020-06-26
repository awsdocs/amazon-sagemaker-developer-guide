# How XGBoost Works<a name="xgboost-HowItWorks"></a>

[XGBoost](https://github.com/dmlc/xgboost) is a popular and efficient open\-source implementation of the gradient boosted trees algorithm\. Gradient boosting is a supervised learning algorithm, which attempts to accurately predict a target variable by combining the estimates of a set of simpler, weaker models\.

When using [gradient boosting](https://en.wikipedia.org/wiki/Gradient_boosting) for regression, the weak learners are regression trees, and each regression tree maps an input data point to one of its leafs that contains a continuous score\. XGBoost minimizes a regularized \(L1 and L2\) objective function that combines a convex loss function \(based on the difference between the predicted and target outputs\) and a penalty term for model complexity \(in other words, the regression tree functions\)\. The training proceeds iteratively, adding new trees that predict the residuals or errors of prior trees that are then combined with previous trees to make the final prediction\. It's called gradient boosting because it uses a gradient descent algorithm to minimize the loss when adding new models\.

**For more detail on XGBoost, see:**
+ [XGBoost: A Scalable Tree Boosting System](https://arxiv.org/pdf/1603.02754.pdf)
+ [Introduction to Boosted Trees](https://xgboost.readthedocs.io/en/latest/tutorials/model.html)