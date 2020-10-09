# XGBoost Algorithm<a name="xgboost"></a>

The [XGBoost](https://github.com/dmlc/xgboost) \(eXtreme Gradient Boosting\) is a popular and efficient open\-source implementation of the gradient boosted trees algorithm\. Gradient boosting is a supervised learning algorithm that attempts to accurately predict a target variable by combining an ensemble of estimates from a set of simpler and weaker models\. The XGBoost algorithm performs well in machine learning competitions because of its robust handling of a variety of data types, relationships, distributions, and the variety of hyperparameters that you can fine\-tune\. You can use XGBoost for regression, classification \(binary and multiclass\), and ranking problems\. 

You can use the new release of the XGBoost algorithm either as a Amazon SageMaker built\-in algorithm or as a framework to run training scripts in your local environments\. This implementation has a smaller memory footprint, better logging, improved hyperparameter validation, and an expanded set of metrics than the original versions\. It provides an XGBoost `estimator` that executes a training script in a managed XGBoost environment\. The current release of SageMaker XGBoost is based on the original XGBoost versions 0\.90, 1\.0, and 1\.2\.

## Supported versions<a name="xgboost-supported-versions"></a>
+ Framework \(open source\) mode: 0\.90\-1, 0\.90\-2, 1\.0\-1, 1\.2\-1
+ Algorithm mode: 0\.90\-1, 0\.90\-2, 1\.0\-1, 1\.2\-1

**Note**  
XGBoost 1\.1 is not supported on SageMaker because XGBoost 1\.1 has a broken capability to run prediction when the test input has fewer features than the training data in LIBSVM inputs\. This capability has been restored in XGBoost 1\.2\. Consider using SageMaker XGBoost 1\.2\-1\.

## How to Use SageMaker XGBoost<a name="xgboost-modes"></a>

With SageMaker, you can use XGBoost as a built\-in algorithm or framework\. By using XGBoost as a framework, you have more flexibility and access to more advanced scenarios, such as k\-fold cross\-validation, because you can customize your own training scripts\. 
+ **Use XGBoost as a framework**

  Use XGBoost as a framework to run your customized training scripts that can incorporate additional data processing into your training jobs\. In the following code example, you can find how SageMaker Python SDK provides the XGBoost API as a framework in the same way it provides other framework APIs, such as TensorFlow, MXNet, and PyTorch\.

------
#### [ SageMaker Python SDK v1 ]

  ```
  import boto3
  import sagemaker
  from sagemaker.xgboost.estimator import XGBoost
  from sagemaker.session import s3_input, Session
  
  # initialize hyperparameters
  hyperparameters = {
          "max_depth":"5",
          "eta":"0.2",
          "gamma":"4",
          "min_child_weight":"6",
          "subsample":"0.7",
          "verbose":"1",
          "objective":"reg:linear",
          "num_round":"50"}
  
  # set an output path where the trained model will be saved
  bucket = sagemaker.Session().default_bucket()
  prefix = 'DEMO-xgboost-as-a-framework'
  output_path = 's3://{}/{}/{}/output'.format(bucket, prefix, 'abalone-xgb-framework')
  
  # construct a SageMaker XGBoost estimator
  # specify the entry_point to your xgboost training script
  estimator = XGBoost(entry_point = "your_xgboost_abalone_script.py", 
                      framework_version='1.2-1',
                      hyperparameters=hyperparameters,
                      role=sagemaker.get_execution_role(),
                      train_instance_count=1,
                      train_instance_type='ml.m5.2xlarge',
                      output_path=output_path)
  
  # define the data type and paths to the training and validation datasets
  content_type = "libsvm"
  train_input = s3_input("s3://{}/{}/{}/".format(bucket, prefix, 'train'), content_type=content_type)
  validation_input = s3_input("s3://{}/{}/{}/".format(bucket, prefix, 'validation'), content_type=content_type)
  
  # execute the XGBoost training job
  estimator.fit({'train': train_input, 'validation': validation_input})
  ```

------
#### [ SageMaker Python SDK v2 ]

  ```
  import boto3
  import sagemaker
  from sagemaker.xgboost.estimator import XGBoost
  from sagemaker.session import Session
  from sagemaker.inputs import TrainingInput
  
  # initialize hyperparameters
  hyperparameters = {
          "max_depth":"5",
          "eta":"0.2",
          "gamma":"4",
          "min_child_weight":"6",
          "subsample":"0.7",
          "verbose":"1",
          "objective":"reg:linear",
          "num_round":"50"}
  
  # set an output path where the trained model will be saved
  bucket = sagemaker.Session().default_bucket()
  prefix = 'DEMO-xgboost-as-a-framework'
  output_path = 's3://{}/{}/{}/output'.format(bucket, prefix, 'abalone-xgb-framework')
  
  # construct a SageMaker XGBoost estimator
  # specify the entry_point to your xgboost training script
  estimator = XGBoost(entry_point = "your_xgboost_abalone_script.py", 
                      framework_version='1.2-1',
                      hyperparameters=hyperparameters,
                      role=sagemaker.get_execution_role(),
                      instance_count=1,
                      instance_type='ml.m5.2xlarge',
                      output_path=output_path)
  
  # define the data type and paths to the training and validation datasets
  content_type = "libsvm"
  train_input = TrainingInput("s3://{}/{}/{}/".format(bucket, prefix, 'train'), content_type=content_type)
  validation_input = TrainingInput("s3://{}/{}/{}/".format(bucket, prefix, 'validation'), content_type=content_type)
  
  # execute the XGBoost training job
  estimator.fit({'train': train_input, 'validation': validation_input})
  ```

------

  For an end\-to\-end example of using SageMaker XGBoost as a framework, see [Regression with Amazon SageMaker XGBoost](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_abalone_dist_script_mode.ipynb)
+ **Use XGBoost as a built\-in algorithm**

  Use the XGBoost built\-in algorithm to build an XGBoost training container as shown in the following code example\. You can automatically spot the XGBoost built\-in algorithm image URI using the SageMaker `image_uris.retrieve` API \(or the `get_image_uri` API if using [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) version 1\)\. If you want to ensure if the `image_uris.retrieve` API finds the correct URI, see [ Common parameters for built\-in algorithms ](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html) and look up `xgboost` from the full list of built\-in algorithm image URIs and available regions\.

  After specifying the XGBoost image URI, you can use the XGBoost container to construct an estimator using the SageMaker Estimator API and initiate a training job\. This XGBoost built\-in algorithm mode does not incorporate your own XGBoost training script and runs directly on the input datasets\. 

------
#### [ SageMaker Python SDK v1 ]

  ```
  import sagemaker
  import boto3
  from sagemaker.amazon.amazon_estimator import get_image_uri 
  from sagemaker.session import s3_input, Session
              
  # initialize hyperparameters
  hyperparameters = {
          "max_depth":"5",
          "eta":"0.2",
          "gamma":"4",
          "min_child_weight":"6",
          "subsample":"0.7",
          "objective":"reg:squarederror",
          "num_round":"50"}
  
  # set an output path where the trained model will be saved
  bucket = sagemaker.Session().default_bucket()
  prefix = 'DEMO-xgboost-as-a-built-in-algo'
  output_path = 's3://{}/{}/{}/output'.format(bucket, prefix, 'abalone-xgb-built-in-algo')
  
  # this line automatically looks for the XGBoost image URI and builds an XGBoost container.
  # specify the repo_version depending on your preference.
  xgboost_container = get_image_uri(boto3.Session().region_name,
                            'xgboost', 
                            repo_version='1.2-1')
  
  # construct a SageMaker estimator that calls the xgboost-container
  estimator = sagemaker.estimator.Estimator(image_name=xgboost_container, 
                                            hyperparameters=hyperparameters,
                                            role=sagemaker.get_execution_role(),
                                            train_instance_count=1, 
                                            train_instance_type='ml.m5.2xlarge', 
                                            train_volume_size=5, # 5 GB 
                                            output_path=output_path)
  
  # define the data type and paths to the training and validation datasets
  content_type = "libsvm"
  train_input = s3_input("s3://{}/{}/{}/".format(bucket, prefix, 'train'), content_type=content_type)
  validation_input = s3_input("s3://{}/{}/{}/".format(bucket, prefix, 'validation'), content_type=content_type)
  
  # execute the XGBoost training job
  estimator.fit({'train': train_input, 'validation': validation_input})
  ```

------
#### [ SageMaker Python SDK v2 ]

  ```
  import sagemaker
  import boto3
  from sagemaker import image_uris
  from sagemaker.session import Session
  from sagemaker.inputs import TrainingInput
  
  # initialize hyperparameters
  hyperparameters = {
          "max_depth":"5",
          "eta":"0.2",
          "gamma":"4",
          "min_child_weight":"6",
          "subsample":"0.7",
          "objective":"reg:squarederror",
          "num_round":"50"}
  
  # set an output path where the trained model will be saved
  bucket = sagemaker.Session().default_bucket()
  prefix = 'DEMO-xgboost-as-a-built-in-algo'
  output_path = 's3://{}/{}/{}/output'.format(bucket, prefix, 'abalone-xgb-built-in-algo')
  
  # this line automatically looks for the XGBoost image URI and builds an XGBoost container.
  # specify the repo_version depending on your preference.
  xgboost_container = sagemaker.image_uris.retrieve("xgboost", region, "1.2-1")
  
  # construct a SageMaker estimator that calls the xgboost-container
  estimator = sagemaker.estimator.Estimator(image_uri=xgboost_container, 
                                            hyperparameters=hyperparameters,
                                            role=sagemaker.get_execution_role(),
                                            instance_count=1, 
                                            instance_type='ml.m5.2xlarge', 
                                            volume_size=5, # 5 GB 
                                            output_path=output_path)
  
  # define the data type and paths to the training and validation datasets
  content_type = "libsvm"
  train_input = TrainingInput("s3://{}/{}/{}/".format(bucket, prefix, 'train'), content_type=content_type)
  validation_input = TrainingInput("s3://{}/{}/{}/".format(bucket, prefix, 'validation'), content_type=content_type)
  
  # execute the XGBoost training job
  estimator.fit({'train': train_input, 'validation': validation_input})
  ```

------

  For more information about how to set up the XGBoost as a built\-in algorithm, see the following notebook examples\.
  + [ Managed Spot Training for XGBoost ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_managed_spot_training.ipynb)
  + [ Regression with Amazon SageMaker XGBoost \(Parquet input\) ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_parquet_input_training.ipynb)

## Input/Output Interface for the XGBoost Algorithm<a name="InputOutput-XGBoost"></a>

Gradient boosting operates on tabular data, with the rows representing observations, one column representing the target variable or label, and the remaining columns representing features\. 

The SageMaker implementation of XGBoost supports CSV and libsvm formats for training and inference:
+ For Training ContentType, valid inputs are *text/libsvm* \(default\) or *text/csv*\.
+ For Inference ContentType, valid inputs are *text/libsvm* \(default\) or *text/csv*\.

**Note**  
For CSV training, the algorithm assumes that the target variable is in the first column and that the CSV does not have a header record\.   
For CSV inference, the algorithm assumes that CSV input does not have the label column\.   
For libsvm training, the algorithm assumes that the label is in the first column\. Subsequent columns contain the zero\-based index value pairs for features\. So each row has the format: <label> <index0>:<value0> <index1>:<value1> \.\.\. Inference requests for libsvm might not have labels in the libsvm format\.

This differs from other SageMaker algorithms, which use the protobuf training input format to maintain greater consistency with standard XGBoost data formats\.

For CSV training input mode, the total memory available to the algorithm \(Instance Count \* the memory available in the `InstanceType`\) must be able to hold the training dataset\. For libsvm training input mode, it's not required, but we recommend it\.

SageMaker XGBoost uses the Python pickle module to serialize/deserialize the model, which can be used for saving/loading the model\.

**To use a model trained with SageMaker XGBoost in open source XGBoost**
+ Use the following Python code:

  ```
  import pickle as pkl 
  import tarfile
  import xgboost
  
  t = tarfile.open('model.tar.gz', 'r:gz')
  t.extractall()
  
  model = pkl.load(open(model_file_path, 'rb'))
  
  # prediction with test data
  pred = model.predict(dtest)
  ```

**To differentiate the importance of labelled data points use Instance Weight Supports**
+ SageMaker XGBoost allows customers to differentiate the importance of labelled data points by assigning each instance a weight value\. For *text/libsvm* input, customers can assign weight values to data instances by attaching them after the labels\. For example, `label:weight idx_0:val_0 idx_1:val_1...`\. For *text/csv* input, customers need to turn on the `csv_weights` flag in the parameters and attach weight values in the column after labels\. For example: `label,weight,val_0,val_1,...`\)\.

## EC2 Instance Recommendation for the XGBoost Algorithm<a name="Instance-XGBoost"></a>

SageMaker XGBoost 1\.0\-1 or earlier currently only trains using CPUs\. It is a memory\-bound \(as opposed to compute\-bound\) algorithm\. So, a general\-purpose compute instance \(for example, M5\) is a better choice than a compute\-optimized instance \(for example, C4\)\. Further, we recommend that you have enough total memory in selected instances to hold the training data\. Although it supports the use of disk space to handle data that does not fit into main memory \(the out\-of\-core feature available with the libsvm input mode\), writing cache files onto disk slows the algorithm processing time\.

SageMaker XGBoost version 1\.2 or later supports single\-instance GPU training\. Despite higher per\-instance costs, GPUs train more quickly, making them more cost effective\. To take advantage of GPU training, specify the instance type as one of the GPU instances \(for example, P3\) and set the `tree_method` hyperparameter to `gpu_hist` in your existing XGBoost script\. SageMaker currently does not support multi\-GPU training\.

## XGBoost Sample Notebooks<a name="xgboost-sample-notebooks"></a>

 The following table outlines a variety of sample notebooks that address different use cases of Amazon SageMaker XGBoost algorithm\.


| **Title** | **Description** | 
| --- | --- | 
|  [How to Create a Custom XGBoost container?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/aws_sagemaker_studio/sagemaker_studio_image_build/xgboost_bring_your_own/Batch_Transform_BYO_XGB.ipynb)  |  This notebook shows you how to build a custom XGBoost Container with Amazon SageMaker Batch Transform\.  | 
|  [An Introduction to Feature Processing, Training and Inference](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/advanced_functionality/inference_pipeline_sparkml_xgboost_abalone/inference_pipeline_sparkml_xgboost_abalone.ipynb)  |  This notebook shows you how to build a Machine Learning \(ML\) Pipeline using Spark Feature Transformers and do real\-time inference using Amazon SageMaker Batch Transform\.   | 
|  [Regression with XGBoost using Parquet](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_parquet_input_training.ipynb)  |  This notebook shows you how to use the Abalone dataset in Parquet to train a XGBoost model\.  | 
|  [How to Train and Host a Multiclass Classification Model? ](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/xgboost_mnist/xgboost_mnist.ipynb)  |  This notebook shows how to use the MNIST dataset to train and host a multiclass classification model\.  | 
|  [How to train a Model for Customer Churn Prediction?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_applying_machine_learning/xgboost_customer_churn/xgboost_customer_churn.ipynb)  |  This notebook shows you how to train a model to Predict Mobile Customer Departure in an effort to identify unhappy customers\.  | 
|  [An Introduction to Amazon SageMaker Managed Spot infrastructure for XGBoost Training](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_managed_spot_training.ipynb)  |  This notebook shows you how to use Spot Instances for training with a XGBoost Container\.  | 
|  [How to use Amazon SageMaker Debugger to debug XGBoost Training Jobs?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/xgboost_builtin_rules/xgboost-regression-debugger-rules.ipynb)  |  This notebook shows you how to use Amazon SageMaker Debugger to monitor training jobs to detect inconsistencies\.  | 
|  [How to use Amazon SageMaker Debugger to debug XGBoost Training Jobs in Real\-Time?](https://github.com/awslabs/amazon-sagemaker-examples/blob/master/sagemaker-debugger/xgboost_realtime_analysis/xgboost-realtime-analysis.ipynb)  |  This notebook shows you how to use the MNIST dataset and Amazon SageMaker Debugger to perform real\-time analysis of XGBoost training jobs while training jobs are running\.  | 

For instructions on how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all of the SageMaker samples\. The topic modeling example notebooks using the linear learning algorithm are located in the **Introduction to Amazon algorithms** section\. To open a notebook, choose its **Use** tab and choose **Create copy**\.