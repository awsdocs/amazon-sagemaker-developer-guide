# Training modes and algorithm support<a name="autopilot-model-support-validation"></a>

Amazon SageMaker Autopilot supports different training modes and algorithms to address machine learning problems, report on quality and objective metrics, and to use cross\-validation automatically, when needed\.

## Training modes<a name="autopilot-training-mode"></a>

SageMaker Autopilot can automatically select the training method based on the dataset size, or you can select it manually\. The choices are as follows:
+ **Ensembling** – Autopilot uses the [AutoGluon](https://auto.gluon.ai/stable/tutorials/tabular_prediction/index.html) library to train several base models\. To find the best combination for your dataset, ensemble mode runs 10 trials with different model and meta parameter settings\. Then Autopilot combines these models using a stacking ensemble method to create an optimal predictive model\. For a list of algorithms that Autopilot supports in ensembling mode, see the following **Algorithm support** section\.
+ **Hyperparameter optimization \(HPO\)** – Autopilot finds the best version of a model by tuning hyperparameters using Bayesian optimization or multi\-fidelity optimization while running training jobs on your dataset\. HPO mode selects the algorithms that are most relevant to your dataset and selects the best range of hyperparameters to tune your models\. To tune your models, HPO mode runs up to 100 trials \(default\) to find the optimal hyperparameters settings within the selected range\. If your dataset size is less than 100 MB, Autopilot uses Bayesian optimization\. Autopilot chooses multi\-fidelity optimization if your dataset is larger than 100 MB\.

  In multi\-fidelity optimization, metrics are continuously emitted from the training containers\. A trial that is performing poorly against a selected objective metric is stopped early\. A trial that is performing well is allocated more resources\. 

  For a list of algorithms that Autopilot supports in HPO mode, see the following **Algorithm support** section\. 
+ **Auto** – Based on your dataset size, Autopilot automatically chooses either ensembling mode or HPO mode\. If your dataset is larger than 100 MB, Autopilot chooses HPO\. Otherwise, it chooses ensembling mode\.

**Note**  
For optimal runtime and performance, use ensemble training mode for datasets that are smaller than 100 MB\.

## Algorithm support<a name="autopilot-algorithm-support"></a>

In **HPO mode**, Autopilot supports the following types of machine learning algorithms:
+  [Linear learner](https://docs.aws.amazon.com/sagemaker/latest/dg/linear-learner.html) – A supervised learning algorithm that can solve either classification or regression problems\.
+ [XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) – A supervised learning algorithm that attempts to accurately predict a target variable by combining an ensemble of estimates from a set of simpler and weaker models\.
+ Deep learning algorithm – A multilayer perceptron \(MLP\) and feedforward artificial neural network\. This algorithm can handle data that is not linearly separable\.

**Note**  
You don't need to specify an algorithm to use for your machine learning problem\. Autopilot automatically selects the appropriate algorithm to train\. 

In **ensembling mode**, the following are types of machine learning algorithms that Autopilot supports: 
+ [LightGBM](https://docs.aws.amazon.com/sagemaker/latest/dg/lightgbm.html) – An optimized framework that uses tree\-based algorithms with gradient boosting\. This algorithm uses trees that grow in breadth, rather than depth, and is highly optimized for speed\.
+ [CatBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/catboost.html) – A framework that uses tree\-based algorithms with gradient boosting\. Optimized for handling categorical variables\.
+ [XGBoost](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html) – A framework that uses tree\-based algorithms with gradient boosting that grows in depth, rather than breadth\. 
+ [Random Forest](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html) – A tree\-based algorithm that uses several decision trees on random sub\-samples of the data with replacement\. The trees are split into optimal nodes at each level\. The decisions of each tree are averaged together to prevent overfitting and improve predictions\.
+ [Extra Trees](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.ExtraTreesClassifier.html#sklearn.ensemble.ExtraTreesClassifier) – A tree\-based algorithm that uses several decision trees on the entire dataset\. The trees are split randomly at each level\. The decisions of each tree are averaged to prevent overfitting and to improve predictions\. Extra trees add a degree of randomization in comparison to the random forest algorithm\.
+ [Linear Models](https://scikit-learn.org/stable/modules/classes.html#module-sklearn.linear_model) – A framework that uses a linear equation to model the relationship between two variables in observed data\.
+ Neural network torch – A neural network model that's implemented using [Pytorch](https://pytorch.org/)\.
+ Neural network fast\.ai – A neural network model that's implemented using [fast\.ai](https://www.fast.ai/)\.