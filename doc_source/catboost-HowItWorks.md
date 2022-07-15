# How CatBoost Works<a name="catboost-HowItWorks"></a>

CatBoost implements a conventional Gradient Boosting Decision Tree \(GBDT\) algorithm with the addition of two critical algorithmic advances:

1. The implementation of ordered boosting, a permutation\-driven alternative to the classic algorithm

1. An innovative algorithm for processing categorical features

Both techniques were created to fight a prediction shift caused by a special kind of target leakage present in all currently existing implementations of gradient boosting algorithms\.

The CatBoost algorithm performs well in machine learning competitions because of its robust handling of a variety of data types, relationships, distributions, and the diversity of hyperparameters that you can fine\-tune\. You can use CatBoost for regression, classification \(binary and multiclass\), and ranking problems\.

For more information on gradient boosting, see [How XGBoost Works](xgboost-HowItWorks.md)\. For in\-depth details about the additional GOSS and EFB techniques used in the CatBoost method, see *[CatBoost: unbiased boosting with categorical features](https://arxiv.org/pdf/1706.09516.pdf)*\.