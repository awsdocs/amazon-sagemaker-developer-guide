# XGBoost Algorithm<a name="xgboost"></a>

The [XGBoost](https://github.com/dmlc/xgboost) \(eXtreme Gradient Boosting\) is a popular and efficient open\-source implementation of the gradient boosted trees algorithm\. Gradient boosting is a supervised learning algorithm that attempts to accurately predict a target variable by combining an ensemble of estimates from a set of simpler and weaker models\. The XGBoost algorithm performs well in machine learning competitions because of its robust handling of a variety of data types, relationships, distributions, and the variety of hyperparameters that you can fine\-tune\. You can use XGBoost for regression, classification \(binary and multiclass\), and ranking problems\. 

You can use the new release of the XGBoost algorithm either as a Amazon SageMaker built\-in algorithm or as a framework to run training scripts in your local environments\. This implementation has a smaller memory footprint, better logging, improved hyperparameter validation, and an expanded set of metrics than the original versions\. It provides an XGBoost `estimator` that executes a training script in a managed XGBoost environment\. The current release of SageMaker XGBoost is based on the original XGBoost versions 1\.0, 1\.2, 1\.3, and 1\.5\.

## Supported versions<a name="xgboost-supported-versions"></a>
+ Framework \(open source\) mode: 1\.0\-1, 1\.2\-1, 1\.2\-2, 1\.3\-1, 1\.5\-1
+ Algorithm mode: 1\.0\-1, 1\.2\-1, 1\.2\-2, 1\.3\-1, 1\.5\-1

**Important**  
When you retrieve the SageMaker XGBoost image URI, do not use `:latest` or `:1` for the image URI tag\. You must specify one of the [Supported versions](#xgboost-supported-versions) to choose the SageMaker\-managed XGBoost container with the native XGBoost package version that you want to use\. To find the package version migrated into the SageMaker XGBoost containers, see [Docker Registry Paths and Example Code](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html), choose your AWS Region, and navigate to the **XGBoost \(algorithm\)** section\.

**Warning**  
The XGBoost 0\.90 versions are deprecated\. Supports for security updates or bug fixes for XGBoost 0\.90 is discontinued\. It is highly recommended to upgrade the XGBoost version to one of the newer versions\.

**Note**  
XGBoost v1\.1 is not supported on SageMaker because XGBoost 1\.1 has a broken capability to run prediction when the test input has fewer features than the training data in LIBSVM inputs\. This capability has been restored in XGBoost v1\.2\. Consider using SageMaker XGBoost 1\.2\-2 or later\.

## How to Use SageMaker XGBoost<a name="xgboost-modes"></a>

With SageMaker, you can use XGBoost as a built\-in algorithm or framework\. By using XGBoost as a framework, you have more flexibility and access to more advanced scenarios, such as k\-fold cross\-validation, because you can customize your own training scripts\. The following sections describe how to use XGBoost with the SageMaker Python SDK\. For information on how to use XGBoost from the Amazon SageMaker Studio UI, see [SageMaker JumpStart](studio-jumpstart.md)\.
+ **Use XGBoost as a framework**

  Use XGBoost as a framework to run your customized training scripts that can incorporate additional data processing into your training jobs\. In the following code example, you can find how SageMaker Python SDK provides the XGBoost API as a framework in the same way it provides other framework APIs, such as TensorFlow, MXNet, and PyTorch\.

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
          "verbosity":"1",
          "objective":"reg:squarederror",
          "num_round":"50"}
  
  # set an output path where the trained model will be saved
  bucket = sagemaker.Session().default_bucket()
  prefix = 'DEMO-xgboost-as-a-framework'
  output_path = 's3://{}/{}/{}/output'.format(bucket, prefix, 'abalone-xgb-framework')
  
  # construct a SageMaker XGBoost estimator
  # specify the entry_point to your xgboost training script
  estimator = XGBoost(entry_point = "your_xgboost_abalone_script.py", 
                      framework_version='1.5-1',
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

  For an end\-to\-end example of using SageMaker XGBoost as a framework, see [Regression with Amazon SageMaker XGBoost](https://sagemaker-examples.readthedocs.io/en/latest/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_abalone_dist_script_mode.html)
+ **Use XGBoost as a built\-in algorithm**

  Use the XGBoost built\-in algorithm to build an XGBoost training container as shown in the following code example\. You can automatically spot the XGBoost built\-in algorithm image URI using the SageMaker `image_uris.retrieve` API \(or the `get_image_uri` API if using [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) version 1\)\. If you want to ensure if the `image_uris.retrieve` API finds the correct URI, see [ Common parameters for built\-in algorithms ](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html) and look up `xgboost` from the full list of built\-in algorithm image URIs and available regions\.

  After specifying the XGBoost image URI, you can use the XGBoost container to construct an estimator using the SageMaker Estimator API and initiate a training job\. This XGBoost built\-in algorithm mode does not incorporate your own XGBoost training script and runs directly on the input datasets\. 
**Important**  
When you retrieve the SageMaker XGBoost image URI, do not use `:latest` or `:1` for the image URI tag\. You must specify one of the [Supported versions](#xgboost-supported-versions) to choose the SageMaker\-managed XGBoost container with the native XGBoost package version that you want to use\. To find the package version migrated into the SageMaker XGBoost containers, see [Docker Registry Paths and Example Code](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-algo-docker-registry-paths.html), choose your AWS Region, and navigate to the **XGBoost \(algorithm\)** section\.

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
  xgboost_container = sagemaker.image_uris.retrieve("xgboost", region, "1.5-1")
  
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

  For more information about how to set up the XGBoost as a built\-in algorithm, see the following notebook examples\.
  + [ Managed Spot Training for XGBoost ](https://sagemaker-examples.readthedocs.io/en/latest/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_managed_spot_training.html)
  + [ Regression with Amazon SageMaker XGBoost \(Parquet input\) ](https://sagemaker-examples.readthedocs.io/en/latest/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_parquet_input_training.html)

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

For v1\.3\-1 and later, SageMaker XGBoost saves the model in the XGBoost internal binary format, using `Booster.save_model`\. Previous versions use the Python pickle module to serialize/deserialize the model\.

**Note**  
Be mindful of versions when using an SageMaker XGBoost model in open source XGBoost\. Versions 1\.3\-1 and later use the XGBoost internal binary format while previous versions use the Python pickle module\.

**To use a model trained with SageMaker XGBoost v1\.3\-1 or later in open source XGBoost**
+ Use the following Python code:

  ```
  import xgboost as xgb
  
  xgb_model = xgb.Booster()
  xgb_model.load_model(model_file_path)
  xgb_model.predict(dtest)
  ```

**To use a model trained with previous versions of SageMaker XGBoost in open source XGBoost**
+ Use the following Python code:

  ```
  import pickle as pkl 
  import tarfile
  
  t = tarfile.open('model.tar.gz', 'r:gz')
  t.extractall()
  
  model = pkl.load(open(model_file_path, 'rb'))
  
  # prediction with test data
  pred = model.predict(dtest)
  ```

**To differentiate the importance of labelled data points use Instance Weight Supports**
+ SageMaker XGBoost allows customers to differentiate the importance of labelled data points by assigning each instance a weight value\. For *text/libsvm* input, customers can assign weight values to data instances by attaching them after the labels\. For example, `label:weight idx_0:val_0 idx_1:val_1...`\. For *text/csv* input, customers need to turn on the `csv_weights` flag in the parameters and attach weight values in the column after labels\. For example: `label,weight,val_0,val_1,...`\)\.

## EC2 Instance Recommendation for the XGBoost Algorithm<a name="Instance-XGBoost"></a>

SageMaker XGBoost 1\.0\-1 or earlier only trains using CPUs\. It is a memory\-bound \(as opposed to compute\-bound\) algorithm\. So, a general\-purpose compute instance \(for example, M5\) is a better choice than a compute\-optimized instance \(for example, C4\)\. Further, we recommend that you have enough total memory in selected instances to hold the training data\. Although it supports the use of disk space to handle data that does not fit into main memory \(the out\-of\-core feature available with the libsvm input mode\), writing cache files onto disk slows the algorithm processing time\.

SageMaker XGBoost version 1\.2 or later supports single\-instance GPU training\. Despite higher per\-instance costs, GPUs train more quickly, making them more cost effective\. SageMaker XGBoost version 1\.2 or later supports P2 and P3 instances\.

SageMaker XGBoost version 1\.2\-2 or later supports P2, P3, G4dn, and G5 GPU instance families\.

To take advantage of GPU training, specify the instance type as one of the GPU instances \(for example, P3\) and set the `tree_method` hyperparameter to `gpu_hist` in your existing XGBoost script\. SageMaker XGBoost currently does not support multi\-GPU training\.

SageMaker XGBoost supports CPU and GPU instances for inference\. For information about the instance types for inference, see [Amazon SageMaker ML Instance Types](https://aws.amazon.com/sagemaker/pricing/instance-types/)\.

## XGBoost Sample Notebooks<a name="xgboost-sample-notebooks"></a>

 The following table outlines a variety of sample notebooks that address different use cases of Amazon SageMaker XGBoost algorithm\.


****  

| **Notebook Title** | **Description** | 
| --- | --- | 
|  [How to Create a Custom XGBoost container?](https://sagemaker-examples.readthedocs.io/en/latest/aws_sagemaker_studio/sagemaker_studio_image_build/xgboost_bring_your_own/Batch_Transform_BYO_XGB.html)  |  This notebook shows you how to build a custom XGBoost Container with Amazon SageMaker Batch Transform\.  | 
|  [Regression with XGBoost using Parquet](https://sagemaker-examples.readthedocs.io/en/latest/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_parquet_input_training.html)  |  This notebook shows you how to use the Abalone dataset in Parquet to train a XGBoost model\.  | 
|  [How to Train and Host a Multiclass Classification Model? ](https://sagemaker-examples.readthedocs.io/en/latest/introduction_to_amazon_algorithms/xgboost_mnist/xgboost_mnist.html)  |  This notebook shows how to use the MNIST dataset to train and host a multiclass classification model\.  | 
|  [How to train a Model for Customer Churn Prediction?](https://sagemaker-examples.readthedocs.io/en/latest/introduction_to_applying_machine_learning/xgboost_customer_churn/xgboost_customer_churn.html)  |  This notebook shows you how to train a model to Predict Mobile Customer Departure in an effort to identify unhappy customers\.  | 
|  [An Introduction to Amazon SageMaker Managed Spot infrastructure for XGBoost Training](https://sagemaker-examples.readthedocs.io/en/latest/introduction_to_amazon_algorithms/xgboost_abalone/xgboost_managed_spot_training.html)  |  This notebook shows you how to use Spot Instances for training with a XGBoost Container\.  | 
|  [How to use Amazon SageMaker Debugger to debug XGBoost Training Jobs?](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-debugger/xgboost_builtin_rules/xgboost-regression-debugger-rules.html)  |  This notebook shows you how to use Amazon SageMaker Debugger to monitor training jobs to detect inconsistencies\.  | 
|  [How to use Amazon SageMaker Debugger to debug XGBoost Training Jobs in Real\-Time?](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-debugger/xgboost_realtime_analysis/xgboost-realtime-analysis.html)  |  This notebook shows you how to use the MNIST dataset and Amazon SageMaker Debugger to perform real\-time analysis of XGBoost training jobs while training jobs are running\.  | 

For instructions on how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all of the SageMaker samples\. The topic modeling example notebooks using the linear learning algorithm are located in the **Introduction to Amazon algorithms** section\. To open a notebook, choose its **Use** tab and choose **Create copy**\.