# TabTransformer<a name="tabtransformer"></a>

[TabTransformer](https://arxiv.org/abs/2012.06678) is a novel deep tabular data modeling architecture for supervised learning\. The TabTransformer architecture is built on self\-attention\-based Transformers\. The Transformer layers transform the embeddings of categorical features into robust contextual embeddings to achieve higher prediction accuracy\. Furthermore, the contextual embeddings learned from TabTransformer are highly robust against both missing and noisy data features, and provide better interpretability\.

## How to use SageMaker TabTransformer<a name="tabtransformer-modes"></a>

You can use TabTransformer as an Amazon SageMaker built\-in algorithm\. The following section describes how to use TabTransformer with the SageMaker Python SDK\. For information on how to use TabTransformer from the Amazon SageMaker Studio UI, see [SageMaker JumpStart](studio-jumpstart.md)\.
+ **Use TabTransformer as a built\-in algorithm**

  Use the TabTransformer built\-in algorithm to build a TabTransformer training container as shown in the following code example\. You can automatically spot the TabTransformer built\-in algorithm image URI using the SageMaker `image_uris.retrieve` API \(or the `get_image_uri` API if using [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) version 2\)\. 

  After specifying the TabTransformer image URI, you can use the TabTransformer container to construct an estimator using the SageMaker Estimator API and initiate a training job\. The TabTransformer built\-in algorithm runs in script mode, but the training script is provided for you and there is no need to replace it\. If you have extensive experience using script mode to create a SageMaker training job, then you can incorporate your own TabTransformer training scripts\.

  ```
  from sagemaker import image_uris, model_uris, script_uris
  
  train_model_id, train_model_version, train_scope = "pytorch-tabtransformerclassification-model", "*", "training"
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
  training_data_prefix = "training-datasets/tabular_multiclass/"
  
  training_dataset_s3_path = f"s3://{training_data_bucket}/{training_data_prefix}"
  
  output_bucket = sess.default_bucket()
  output_prefix = "jumpstart-example-tabular-training"
  
  s3_output_location = f"s3://{output_bucket}/{output_prefix}/output"
  
  from sagemaker import hyperparameters
  
  # Retrieve the default hyper-parameters for training the model
  hyperparameters = hyperparameters.retrieve_default(
      model_id=train_model_id, model_version=train_model_version
  )
  
  # [Optional] Override default hyperparameters with custom values
  hyperparameters[
      "n_epochs"
  ] = "50"
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
      {"training": training_dataset_s3_path}, logs=True, job_name=training_job_name
  )
  ```

  For more information about how to set up the TabTransformer as a built\-in algorithm, see the following notebook examples\.
  + [Tabular classification with Amazon SageMaker TabTransformer algorithm](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/tabtransformer_tabular/Amazon_Tabular_Classification_TabTransformer.ipynb)
  + [Tabular regression with Amazon SageMaker TabTransformer algorithm](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/tabtransformer_tabular/Amazon_Tabular_Regression_TabTransformer.ipynb)

## Input and Output interface for the TabTransformer algorithm<a name="InputOutput-TabTransformer"></a>

TabTransformer operates on tabular data, with the rows representing observations, one column representing the target variable or label, and the remaining columns representing features\. 

The SageMaker implementation of TabTransformer supports CSV for training and inference:
+ For **Training ContentType**, valid inputs must be *text/csv*\.
+ For **Inference ContentType**, valid inputs must be *text/csv*\.

**Note**  
For CSV training, the algorithm assumes that the target variable is in the first column and that the CSV does not have a header record\.   
For CSV inference, the algorithm assumes that CSV input does not have the label column\. 

Be mindful of how to format your training data for input to the TabTransformer model\. You must provide the path to an Amazon S3 bucket containing subdirectories for your training and optional validation data\. You can also include a list of categorical features\.
+ **Training data input format**: Your training data should be in a subdirectory named `train/` that contains a `data.csv` file\. The target variables should be in the first column of `data.csv`\. The predictor variables \(features\) should be in the remaining columns\.
+ **Validation data input format**: You can optionally include another directory called `validation/` that also has a `data.csv` file\. The validation data is used to compute a validation score at the end of each boosting iteration\. Early stopping is applied when the validation score stops improving\. If the validation data is not provided, then 20% of your training data is randomly sampled to serve as the validation data\. 
+ **Categorical features input format**: If your predictors include categorical features, you can provide a JSON file named `categorical_index.json` in the same location as your data directories\. This file should contain a Python dictionary where the key is the string `"cat_index_list"` and the value is a list of unique integers\. Each integer in the value list should indicate the column index of the corresponding categorical features in your training data CSV file\. Each value should be a positive integer \(greater than zero because zero represents the target value\), less than the `Int32.MaxValue` \(2147483647\), and less than the total number of columns\. There should only be one categorical index JSON file\.

For CSV training input mode, the total memory available to the algorithm \(instance count multiplied by the memory available in the `InstanceType`\) must be able to hold the training dataset\.

## Amazon EC2 instance recommendation for the TabTransformer algorithm<a name="Instance-TabTransformer"></a>

SageMaker TabTransformer supports single\-instance CPU and single\-instance GPU training\. Despite higher per\-instance costs, GPUs train more quickly, making them more cost effective\. To take advantage of GPU training, specify the instance type as one of the GPU instances \(for example, P3\)\. SageMaker TabTransformer currently does not support multi\-GPU training\.

## TabTransformer sample notebooks<a name="tabtransformer-sample-notebooks"></a>

The following table outlines a variety of sample notebooks that address different use cases of Amazon SageMaker TabTransformer algorithm\.


****  

| **Notebook Title** | **Description** | 
| --- | --- | 
|  [Tabular classification with Amazon SageMaker TabTransformer algorithm](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/tabtransformer_tabular/Amazon_Tabular_Classification_TabTransformer.ipynb)  |  This notebook demonstrates the use of the Amazon SageMaker TabTransformer algorithm to train and host a tabular classification model\.   | 
|  [Tabular regression with Amazon SageMaker TabTransformer algorithm](https://github.com/aws/amazon-sagemaker-examples/blob/main/introduction_to_amazon_algorithms/tabtransformer_tabular/Amazon_Tabular_Regression_TabTransformer.ipynb)  |  This notebook demonstrates the use of the Amazon SageMaker TabTransformer algorithm to train and host a tabular regression model\.   | 

For instructions on how to create and access Jupyter notebook instances that you can use to run the example in SageMaker, see [Use Amazon SageMaker Notebook Instances](nbi.md)\. After you have created a notebook instance and opened it, choose the **SageMaker Examples** tab to see a list of all of the SageMaker samples\. To open a notebook, choose its **Use** tab and choose **Create copy**\.