# AutoGluon\-Tabular<a name="autogluon-tabular"></a>

[AutoGluon\-Tabular](https://auto.gluon.ai/stable/index.html) is a popular open\-source AutoML framework that trains highly accurate machine learning models on an unprocessed tabular dataset\. Unlike existing AutoML frameworks that primarily focus on model and hyperparameter selection, AutoGluon\-Tabular succeeds by ensembling multiple models and stacking them in multiple layers\. 

## How to use SageMaker AutoGluon\-Tabular<a name="autogluon-tabular-modes"></a>

You can use AutoGluon\-Tabular as an Amazon SageMaker built\-in algorithm\. The following section describes how to use AutoGluon\-Tabular with the SageMaker Python SDK\. For information on how to use AutoGluon\-Tabular from the Amazon SageMaker Studio UI, see [SageMaker JumpStart](studio-jumpstart.md)\.
+ **Use AutoGluon\-Tabular as a built\-in algorithm**

  Use the AutoGluon\-Tabular built\-in algorithm to build an AutoGluon\-Tabular training container as shown in the following code example\. You can automatically spot the AutoGluon\-Tabular built\-in algorithm image URI using the SageMaker `image_uris.retrieve` API \(or the `get_image_uri` API if using [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) version 2\)\. 

  After specifying the AutoGluon\-Tabular image URI, you can use the AutoGluon\-Tabular container to construct an estimator using the SageMaker Estimator API and initiate a training job\. The AutoGluon\-Tabular built\-in algorithm runs in script mode, but the training script is provided for you and there is no need to replace it\. If you have extensive experience using script mode to create a SageMaker training job, then you can incorporate your own AutoGluon\-Tabular training scripts\.

  ```
  from sagemaker import image_uris, model_uris, script_uris
  
  train_model_id, train_model_version, train_scope = "autogluon-classification-ensemble", "*", "training"
  training_instance_type = "ml.p3.2xlarge"
  
  # Retrieve the docker image
  train_image_uri = image_uris.retrieve(
      region=None,
      framework=None,
      model_id=train_model_id,
      model_version=train_model_version,
      image_scope=train_scope,
      instance_type=training_instance_type
  )
  
  # Retrieve the training script
  train_source_uri = script_uris.retrieve(
      model_id=train_model_id, model_version=train_model_version, script_scope=train_scope
  )
  
  train_model_uri = model_uris.retrieve(
      model_id=train_model_id, model_version=train_model_version, model_scope=train_scope
  )
  
  # Sample training data is available in this bucket
  training_data_bucket = f"jumpstart-cache-prod-{aws_region}"
  training_data_prefix = "training-datasets/tabular_binary/"
  
  training_dataset_s3_path = f"s3://{training_data_bucket}/{training_data_prefix}/train"
  validation_dataset_s3_path = f"s3://{training_data_bucket}/{training_data_prefix}/validation"
  
  output_bucket = sess.default_bucket()
  output_prefix = "jumpstart-example-tabular-training"
  
  s3_output_location = f"s3://{output_bucket}/{output_prefix}/output"
  
  from sagemaker import hyperparameters
  
  # Retrieve the default hyperparameters for training the model
  hyperparameters = hyperparameters.retrieve_default(
      model_id=train_model_id, model_version=train_model_version
  )
  
  # [Optional] Override default hyperparameters with custom values
  hyperparameters[
      "auto_stack"
  ] = "True"
  print(hyperparameters)
  
  from sagemaker.estimator import Estimator
  from sagemaker.utils import name_from_base
  
  training_job_name = name_from_base(f"built-in-algo-{train_model_id}-training")
  
  # Create SageMaker Estimator instance
  tabular_estimator = Estimator(
      role=aws_role,
      image_uri=train_image_uri,
      source_dir=train_source_uri,
      model_uri=train_model_uri,
      entry_point="transfer_learning.py",
      instance_count=1,
      instance_type=training_instance_type,
      max_run=360000,
      hyperparameters=hyperparameters,
      output_path=s3_output_location
  )
  
  # Launch a SageMaker Training job by passing the S3 path of the training data
  tabular_estimator.fit(
      {
          "training": training_dataset_s3_path,
          "validation": validation_dataset_s3_path,
      }, logs=True, job_name=training_job_name
  )
  ```

  For more information about how to set up the AutoGluon\-Tabular as a built\-in algorithm, see the following notebook examples\. Any S3 bucket used in these examples must be in the same AWS Region as the notebook instance used to run them\.
  + [Tabular classification with Amazon SageMaker AutoGluon\-Tabular algorithm](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/autogluon_tabular/Amazon_Tabular_Classification_AutoGluon.ipynb)
  + [Tabular regression with Amazon SageMaker AutoGluon\-Tabular algorithm](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/autogluon_tabular/Amazon_Tabular_Regression_AutoGluon.ipynb)

## Input and Output interface for the AutoGluon\-Tabular algorithm<a name="InputOutput-AutoGluon-Tabular"></a>

Gradient boosting operates on tabular data, with the rows representing observations, one column representing the target variable or label, and the remaining columns representing features\. 

The SageMaker implementation of AutoGluon\-Tabular supports CSV for training and inference:
+ For **Training ContentType**, valid inputs must be *text/csv*\.
+ For **Inference ContentType**, valid inputs must be *text/csv*\.

**Note**  
For CSV training, the algorithm assumes that the target variable is in the first column and that the CSV does not have a header record\.   
For CSV inference, the algorithm assumes that CSV input does not have the label column\. 

**Input format for training data, validation data, and categorical features**

Be mindful of how to format your training data for input to the AutoGluon\-Tabular model\. You must provide the path to an Amazon S3 bucket that contains your training and validation data\. You can also include a list of categorical features\. Use both the `training` and `validation` channels to provide your input data\. Alternatively, you can use only the `training` channel\.

**Use both the `training` and `validation` channels**

You can provide your input data by way of two S3 paths, one for the `training` channel and one for the `validation` channel\. Each S3 path can either be an S3 prefix that points to one or more CSV files or a full S3 path pointing to one specific CSV file\. The target variables should be in the first column of your CSV file\. The predictor variables \(features\) should be in the remaining columns\. If multiple CSV files are provided for the `training` or `validation` channels, the AutoGluon\-Tabular algorithm concatenates the files\. The validation data is used to compute a validation score at the end of each boosting iteration\. Early stopping is applied when the validation score stops improving\.

If your predictors include categorical features, you can provide a JSON file named `categorical_index.json` in the same location as your training data file or files\. If you provide a JSON file for categorical features, your `training` channel must point to an S3 prefix and not a specific CSV file\. This file should contain a Python dictionary where the key is the string `"cat_index_list"` and the value is a list of unique integers\. Each integer in the value list should indicate the column index of the corresponding categorical features in your training data CSV file\. Each value should be a positive integer \(greater than zero because zero represents the target value\), less than the `Int32.MaxValue` \(2147483647\), and less than the total number of columns\. There should only be one categorical index JSON file\.

**Use only the `training` channel**:

You can alternatively provide your input data by way of a single S3 path for the `training` channel\. This S3 path should point to a directory with a subdirectory named `training/` that contains one or more CSV files\. You can optionally include another subdirectory in the same location called `validation/` that also has one or more CSV files\. If the validation data is not provided, then 20% of your training data is randomly sampled to serve as the validation data\. If your predictors include categorical features, you can provide a JSON file named `categorical_index.json` in the same location as your data subdirectories\.

**Note**  
For CSV training input mode, the total memory available to the algorithm \(instance count multiplied by the memory available in the `InstanceType`\) must be able to hold the training dataset\.

SageMaker AutoGluon\-Tabular uses the `autogluon.tabular.TabularPredictor` module to serialize or deserialize the model, which can be used for saving or loading the model\.

**To use a model trained with SageMaker AutoGluon\-Tabular with the AutoGluon framework**
+ Use the following Python code:

  ```
  import tarfile
  from autogluon.tabular import TabularPredictor
  
  t = tarfile.open('model.tar.gz', 'r:gz')
  t.extractall()
  
  model = TabularPredictor.load(model_file_path)
  
  # prediction with test data
  # dtest should be a pandas DataFrame with column names feature_0, feature_1, ..., feature_d
  pred = model.predict(dtest)
  ```

## Amazon EC2 instance recommendation for the AutoGluon\-Tabular algorithm<a name="Instance-AutoGluon-Tabular"></a>

SageMaker AutoGluon\-Tabular supports single\-instance CPU and single\-instance GPU training\. Despite higher per\-instance costs, GPUs train more quickly, making them more cost effective\. To take advantage of GPU training, specify the instance type as one of the GPU instances \(for example, P3\)\. SageMaker AutoGluon\-Tabular currently does not support multi\-GPU training\.

## AutoGluon\-Tabular sample notebooks<a name="autogluon-tabular-sample-notebooks"></a>

 The following table outlines a variety of sample notebooks that address different use cases of Amazon SageMaker AutoGluon\-Tabular algorithm\.


****  

| **Notebook Title** | **Description** | 
| --- | --- | 
|  [Tabular classification with Amazon SageMaker AutoGluon\-Tabular algorithm](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/autogluon_tabular/Amazon_Tabular_Classification_AutoGluon.ipynb)  |  This notebook demonstrates the use of the Amazon SageMaker AutoGluon\-Tabular algorithm to train and host a tabular classification model\.  | 
|  [Tabular regression with Amazon SageMaker AutoGluon\-Tabular algorithm](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/autogluon_tabular/Amazon_Tabular_Regression_AutoGluon.ipynb)  |  This notebook demonstrates the use of the Amazon SageMaker AutoGluon\-Tabular algorithm to train and host a tabular regression model\.  | 

For instructions on how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all of the SageMaker samples\. To open a notebook, choose its **Use** tab and choose **Create copy**\.