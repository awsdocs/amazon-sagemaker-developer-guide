# XGBoost Algorithm<a name="xgboost"></a>

[XGBoost](https://github.com/dmlc/xgboost) \(eXtreme Gradient Boosting\) is a popular and efficient open\-source implementation of the gradient boosted trees algorithm\. Gradient boosting is a supervised learning algorithm that attempts to accurately predict a target variable by combining the estimates of a set of simpler, weaker models\. XGBoost has done remarkably well in machine learning competitions because to it robustly handles a variety of data types, relationships, and distributions, and the large number of hyperparameters that can be tweaked and tuned for improved fits\. This flexibility makes XGBoost a solid choice for problems in regression, classification \(binary and multiclass\), and ranking\.

## Input/Output Interface<a name="InputOutput-XGBoost"></a>

Gradient boosting operates on tabular data, with the rows representing observations, one column representing the target variable or label, and the remaining columns representing features\. 

Amazon SageMaker’s implementation of XGBoost supports CSV and libsvm formats for training and inference:
+ For Training ContentType, valid inputs are *libsvm* or *csv*\.
+ For Inference ContentType, valid inputs are `text/x-libsvm` or `text/csv`\.

**Note**  
For CSV training, the algorithm assumes that the target variable is in the first column and that the CSV does not have a header record\. For CSV inference, the algorithm assumes that CSV input does not have the label column\. 

This differs from other Amazon SageMaker algorithms, which use the protobuf training input format to maintain greater consistency with standard XGBoost data formats\.

For CSV training input mode, the total memory available to the algorithm \(Instance Count \* the memory available in the `InstanceType`\) must be able to hold the training dataset\. For libsvm training input mode, it's not required, but we recommend it\.

SageMaker XGBoost uses the Python pickle module to serialize/deserialize the model, which can be used for saving/loading the model\.

**To use a model trained with SageMaker XGBoost in open source XGBoost**
+ Use the following Python code:

  ```
  import pickle as pkl 
  model = pkl.load(open(model_file_path, 'rb'))
  # prediction with test data
  pred = model.predict(dtest)
  ```

## EC2 Instance Recommendation<a name="Instance-XGBoost"></a>

Amazon SageMaker XGBoost currently only trains using CPUs\. It is a memory\-bound \(as opposed to compute\-bound\) algorithm\. So, a general\-purpose compute instance \(for example, M4\) is a better choice than a compute\-optimized instance \(for example, C4\)\. Further, we recommend that you have enough total memory in selected instances to hold the training data\. Although it supports the use of disk space to handle data that does not fit into main memory \(the out\-of\-core feature available with the libsvm input mode\), writing cache files onto disk slows the algorithm processing time\.

**Topics**
+ [Input/Output Interface](#InputOutput-XGBoost)
+ [EC2 Instance Recommendation](#Instance-XGBoost)
+ [How XGBoost Works](xgboost-HowItWorks.md)
+ [XGBoost Hyperparameters](xgboost_hyperparameters.md)
+ [Tuning a XGBoost Model](xgboost-tuning.md)