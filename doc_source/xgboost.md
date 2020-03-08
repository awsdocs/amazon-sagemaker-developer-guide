# XGBoost Algorithm<a name="xgboost"></a>

The [XGBoost](https://github.com/dmlc/xgboost) \(eXtreme Gradient Boosting\) is a popular and efficient open\-source implementation of the gradient boosted trees algorithm\. Gradient boosting is a supervised learning algorithm that attempts to accurately predict a target variable by combining an ensemble of estimates from a set of simpler, weaker models\. XGBoost has done remarkably well in machine learning competitions because it robustly handles a variety of data types, relationships, and distributions, and because of the large number of hyperparameters that can be tweaked and tuned for improved fits\. This flexibility makes XGBoost a solid choice for problems in regression, classification \(binary and multiclass\), and ranking\.

This current release of the XGBoost algorithm makes upgrades from the open source XGBoost code base easy to install and use in Amazon SageMaker\. Customers can use this release of the XGBoost algorithm either as an Amazon SageMaker built\-in algorithm, as with the previous 0\.72\-based version, or as a framework to run training scripts in their local environments as they would typically do, for example, with a TensorFlow deep learning framework\. This implementation has a smaller memory footprint, better logging, improved hyperparameter validation, and an expanded set of metrics than the original 0\.72\-based version\. It also provides an XGBoost `estimator` that executes a training script in a managed XGBoost environment\. The current release of Amazon SageMaker XGBoost is based on version 0\.90 and will be upgradeable to future releases\. The previous implementation [XGBoost Release 0\.72](xgboost-72.md) is still available to customers if they need to postpone migrating to the current version\. But this previous implementation will remain tied to the 0\.72 release of XGBoost\.

**Topics**
+ [How to Use Amazon SageMaker XGBoost](#xgboost-modes)
+ [Input/Output Interface for the XGBoost Algorithm](#InputOutput-XGBoost)
+ [EC2 Instance Recommendation for the XGBoost Algorithm](#Instance-XGBoost)
+ [XGBoost Sample Notebooks](#xgboost-sample-notebooks)
+ [How XGBoost Works](xgboost-HowItWorks.md)
+ [XGBoost Hyperparameters](xgboost_hyperparameters.md)
+ [Tune an XGBoost Model](xgboost-tuning.md)
+ [XGBoost Previous Versions](xgboost-previous-versions.md)

## How to Use Amazon SageMaker XGBoost<a name="xgboost-modes"></a>

The XGBoost algorithm can be used as a built\-in algorithm or as a framework such as TensorFlow\. Using XGBoost as a framework provides more flexible than using it as a built\-in algorithm as it enables more advanced scenarios that allow pre\-processing and post\-processing scripts to be incorporated into your training script\. Using XGBoost as a built\-in Amazon SageMaker algorithm is how you had to use the original [XGBoost Release 0\.72](xgboost-72.md) version and nothing changes here except the version of XGBoost that you use\.
+ **Use XGBoost as a framework**

  Use XGBoost as a framework to run scripts that can incorporate additional data processing into your training jobs\. This way of using XGBoost should be to familiar to users who have worked with the open source [XGBoost](https://github.com/dmlc/xgboost) and other Amazon SageMaker frameworks such as Scikit\-learn\. You use the Amazon SageMaker Python SDK as you would for other frameworks such as TensorFlow\. One change from other Amazon SageMaker frameworks is that the `framework_version` field of the `estimator` for `XGBoost` is mandatory and is not set by default\. Note that the first part of the version refers to the upstream module version \(aka, 0\.90\), while the second part refers to the Amazon SageMaker version for the container\. An error is generated if the `framework_version` is not set\.

  ```
  import sagemaker.xgboost
  estimator = XGBoost(entry_point = 'myscript.py', 
                      source_dir, model_dir, 
                      train_instance_type,                    
                      train_instance_count, 
                      hyperparameters, 
                      role, 
                      base_job_name, 
                      framework_version = '0.90-2', 
                      py_version)
  estimator.fit({'train':'s3://my-bucket/training', 
                  'validation':'s3://my-bucket/validation})
  ```

  The AWS SDK for Python \(Boto 3\) and the CLI also require this field\.
+ **Use XGBoost as a built\-in algorithm**

  Use XGBoost to train and deploy a model as you would other built\-in Amazon SageMaker algorithms\. Using the current version of XGBoost as a built\-in algorithm will be familiar to users who have used the original [XGBoost Release 0\.72](xgboost-72.md) version with the Amazon SageMaker Python SDK and want to continue using the same procedures\.

  ```
  import sagemaker
  from sagemaker.amazon.amazon_estimator import get_image_uri 
  # get the URI for new container
  container = get_image_uri(boto3.Session().region_name,
                            'xgboost', 
                            repo_version='0.90-2'); 
  estimator = sagemaker.estimator.Estimator(container, role, instance_count, instance_type, train_volume_size, output_path, sagemaker.Session());
  estimator.fit({'train':'s3://my-bucket/training', 'validation':'s3://my-bucket/validation})
  ```

  If customers do not specify version in the `get_image_uri` function, they get the [XGBoost Release 0\.72](xgboost-72.md) version by default\. If you want to migrate to the current version, you have to specify `repo_version='0.90-2'` in the `get_image_uri` function\. If you use the current version, you must update your code to use the new hyperparameters that are required by the 0\.90 version of upstream algorithm\. The AWS SDK for Python \(Boto 3\) and the CLI usage is similar\. You also have to choose the version you want to run when using the Console to select the XGBoost algorithm\.

## Input/Output Interface for the XGBoost Algorithm<a name="InputOutput-XGBoost"></a>

Gradient boosting operates on tabular data, with the rows representing observations, one column representing the target variable or label, and the remaining columns representing features\. 

The Amazon SageMaker implementation of XGBoost supports CSV and libsvm formats for training and inference:
+ For Training ContentType, valid inputs are *text/libsvm* \(default\) or *text/csv*\.
+ For Inference ContentType, valid inputs are *text/libsvm* or \(the default\) *text/csv*\.

**Note**  
For CSV training, the algorithm assumes that the target variable is in the first column and that the CSV does not have a header record\. For CSV inference, the algorithm assumes that CSV input does not have the label column\.   
For libsvm training, the algorithm assumes that the label is in the first column\. Subsequent columns contain the zero\-based index value pairs for features\. So each row has the format: <label> <index0>:<value0> <index1>:<value1> \.\.\. Inference requests for libsvm may or may not have labels in the libsvm format\.

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

**To differentiate the importance of labelled data points use Instance Weight Supports**
+ Amazon SageMaker XGBoost allows customers to differentiate the importance of labelled data points by assigning each instance a weight value\. For *text/libsvm* input, customers can assign weight values to data instances by attaching them after the labels\. For example, `label:weight idx_0:val_0 idx_1:val_1...`\. For *text/csv* input, customers need to turn on the `csv_weights` flag in the parameters and attach weight values in the column after labels\. For example: `label,weight,val_0,val_1,...`\)\.

## EC2 Instance Recommendation for the XGBoost Algorithm<a name="Instance-XGBoost"></a>

Amazon SageMaker XGBoost currently only trains using CPUs\. It is a memory\-bound \(as opposed to compute\-bound\) algorithm\. So, a general\-purpose compute instance \(for example, M5\) is a better choice than a compute\-optimized instance \(for example, C4\)\. Further, we recommend that you have enough total memory in selected instances to hold the training data\. Although it supports the use of disk space to handle data that does not fit into main memory \(the out\-of\-core feature available with the libsvm input mode\), writing cache files onto disk slows the algorithm processing time\.

## XGBoost Sample Notebooks<a name="xgboost-sample-notebooks"></a>

For a sample notebook that shows how to use Amazon SageMaker XGBoost as a built\-in algorithm to train and host a regression model, see [Regression with the Amazon SageMaker XGBoost algorithm](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_abalone.ipynb)\. For instructions how to create and access Jupyter notebook instances that you can use to run the example in Amazon SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. Once you have created a notebook instance and opened it, select the **SageMaker Examples** tab to see a list of all the Amazon SageMaker samples\. The topic modeling example notebooks using the NTM algorithms are located in the **Introduction to Amazon algorithms** section\. To open a notebook, click on its **Use** tab and select **Create copy**\.