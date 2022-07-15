# Step 4: Train a Model<a name="ex1-train-model"></a>

The [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) provides framework estimators and generic estimators to train your model while orchestrating the machine learning \(ML\) lifecycle accessing the SageMaker features for training and the AWS infrastructures, such as Amazon Elastic Container Registry \(Amazon ECR\), Amazon Elastic Compute Cloud \(Amazon EC2\), Amazon Simple Storage Service \(Amazon S3\)\. For more information about SageMaker built\-in framework estimators, see [Frameworks](https://sagemaker.readthedocs.io/en/stable/frameworks/index.html)in the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) documentation\. For more information about built\-in algorithms, see [Use Amazon SageMaker Built\-in Algorithms or Pre\-trained Models](algos.md)\.

**Topics**
+ [Choose the Training Algorithm](#ex1-train-model-select-algorithm)
+ [Create and Run a Training Job](#ex1-train-model-sdk)

## Choose the Training Algorithm<a name="ex1-train-model-select-algorithm"></a>

To choose the right algorithm for your dataset, you typically need to evaluate different models to find the most suitable models to your data\. For simplicity, the SageMaker [XGBoost Algorithm](xgboost.md) built\-in algorithm is used throughout this tutorial without the pre\-evaluation of models\.

**Tip**  
If you want SageMaker to find an appropriate model for your tabular dataset, use Amazon SageMaker Autopilot that automates a machine learning solution\. For more information, see [Automate model development with Amazon SageMaker Autopilot](autopilot-automate-model-development.md)\.

## Create and Run a Training Job<a name="ex1-train-model-sdk"></a>

After you figured out which model to use, start constructing a SageMaker estimator for training\. This tutorial uses the XGBoost built\-in algorithm for the SageMaker generic estimator\.

**To run a model training job**

1. Import the [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io) and start by retrieving the basic information from your current SageMaker session\.

   ```
   import sagemaker
   
   region = sagemaker.Session().boto_region_name
   print("AWS Region: {}".format(region))
   
   role = sagemaker.get_execution_role()
   print("RoleArn: {}".format(role))
   ```

   This returns the following information:
   + `region` – The current AWS Region where the SageMaker notebook instance is running\.
   + `role` – The IAM role used by the notebook instance\.
**Note**  
Check the SageMaker Python SDK version by running `sagemaker.__version__`\. This tutorial is based on `sagemaker>=2.20`\. If the SDK is outdated, install the latest version by running the following command:   

   ```
   ! pip install -qU sagemaker
   ```
If you run this installation in your exiting SageMaker Studio or notebook instances, you need to manually refresh the kernel to finish applying the version update\.

1. Create an XGBoost estimator using the `sagemaker.estimator.Estimator` class\. In the following example code, the XGBoost estimator is named `xgb_model`\.

   ```
   from sagemaker.debugger import Rule, rule_configs
   from sagemaker.session import TrainingInput
   
   s3_output_location='s3://{}/{}/{}'.format(bucket, prefix, 'xgboost_model')
   
   container=sagemaker.image_uris.retrieve("xgboost", region, "1.2-1")
   print(container)
   
   xgb_model=sagemaker.estimator.Estimator(
       image_uri=container,
       role=role,
       instance_count=1,
       instance_type='ml.m4.xlarge',
       volume_size=5,
       output_path=s3_output_location,
       sagemaker_session=sagemaker.Session(),
       rules=[Rule.sagemaker(rule_configs.create_xgboost_report())]
   )
   ```

   To construct the SageMaker estimator, specify the following parameters:
   + `image_uri` – Specify the training container image URI\. In this example, the SageMaker XGBoost training container URI is specified using `sagemaker.image_uris.retrieve`\.
   + `role` – The AWS Identity and Access Management \(IAM\) role that SageMaker uses to perform tasks on your behalf \(for example, reading training results, call model artifacts from Amazon S3, and writing training results to Amazon S3\)\. 
   + `instance_count` and `instance_type` – The type and number of Amazon EC2 ML compute instances to use for model training\. For this training exercise, you use a single `ml.m4.xlarge` instance, which has 4 CPUs, 16 GB of memory, an Amazon Elastic Block Store \(Amazon EBS\) storage, and a high network performance\. For more information about EC2 compute instance types, see [Amazon EC2 Instance Types](http://aws.amazon.com/ec2/instance-types/)\. For more information about billing, see [Amazon SageMaker pricing](http://aws.amazon.com/sagemaker/pricing/)\. 
   + `volume_size` – The size, in GB, of the EBS storage volume to attach to the training instance\. This must be large enough to store training data if you use `File` mode \(`File` mode is on by default\)\.
   + `output_path` – The path to the S3 bucket where SageMaker stores the model artifact and training results\.
   + `sagemaker_session` – The session object that manages interactions with SageMaker API operations and other AWS service that the training job uses\.
   + `rules` – Specify a list of SageMaker Debugger built\-in rules\. In this example, the `create_xgboost_report()` rule creates an XGBoost report that provides insights into the training progress and results\. For more information, see [SageMaker Debugger XGBoost Training Report](debugger-training-xgboost-report.md)\.
**Tip**  
If you want to run distributed training of large sized deep learning models, such as convolutional neural networks \(CNN\) and natural language processing \(NLP\) models, use SageMaker Distributed for data parallelism or model parallelism\. For more information, see [Amazon SageMaker Distributed Training Libraries](distributed-training.md)\.

1. Set the hyperparameters for the XGBoost algorithm by calling the `set_hyperparameters` method of the estimator\. For a complete list of XGBoost hyperparameters, see [XGBoost Hyperparameters](xgboost_hyperparameters.md)\.

   ```
   xgb_model.set_hyperparameters(
       max_depth = 5,
       eta = 0.2,
       gamma = 4,
       min_child_weight = 6,
       subsample = 0.7,
       objective = "binary:logistic",
       num_round = 1000
   )
   ```
**Tip**  
You can also tune the hyperparameters using the SageMaker hyperparameter optimization feature\. For more information, see [Perform Automatic Model Tuning with SageMaker](automatic-model-tuning.md)\. 

1. Use the `TrainingInput` class to configure a data input flow for training\. The following example code shows how to configure `TrainingInput` objects to use the training and validation datasets you uploaded to Amazon S3 in the [Split the Dataset into Train, Validation, and Test Datasets](ex1-preprocess-data.md#ex1-preprocess-data-transform) section\.

   ```
   from sagemaker.session import TrainingInput
   
   train_input = TrainingInput(
       "s3://{}/{}/{}".format(bucket, prefix, "data/train.csv"), content_type="csv"
   )
   validation_input = TrainingInput(
       "s3://{}/{}/{}".format(bucket, prefix, "data/validation.csv"), content_type="csv"
   )
   ```

1. To start model training, call the estimator's `fit` method with the training and validation datasets\. By setting `wait=True`, the `fit` method displays progress logs and waits until training is complete\.

   ```
   xgb_model.fit({"train": train_input, "validation": validation_input}, wait=True)
   ```

   For more information about model training, see [Train a Model with Amazon SageMaker](how-it-works-training.md)\. This tutorial training job might take up to 10 minutes\.

   After the training job has done, you can download an XGBoost training report and a profiling report generated by SageMaker Debugger\. The XGBoost training report offers you insights into the training progress and results, such as the loss function with respect to iteration, feature importance, confusion matrix, accuracy curves, and other statistical results of training\. For example, you can find the following loss curve from the XGBoost training report which clearly indicates that there is an overfitting problem\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/get-started-ni/gs-ni-train-loss-curve-validation-overfitting.png)

   Run the following code to specify the S3 bucket URI where the Debugger training reports are generated and check if the reports exist\.

   ```
   rule_output_path = xgb_model.output_path + "/" + xgb_model.latest_training_job.job_name + "/rule-output"
   ! aws s3 ls {rule_output_path} --recursive
   ```

   Download the Debugger XGBoost training and profiling reports to the current workspace:

   ```
   ! aws s3 cp {rule_output_path} ./ --recursive
   ```

   Run the following IPython script to get the file link of the XGBoost training report:

   ```
   from IPython.display import FileLink, FileLinks
   display("Click link below to view the XGBoost Training report", FileLink("CreateXgboostReport/xgboost_report.html"))
   ```

   The following IPython script returns the file link of the Debugger profiling report that shows summaries and details of the EC2 instance resource utilization, system bottleneck detection results, and python operation profiling results:

   ```
   profiler_report_name = [rule["RuleConfigurationName"] 
                           for rule in xgb_model.latest_training_job.rule_job_summary() 
                           if "Profiler" in rule["RuleConfigurationName"]][0]
   profiler_report_name
   display("Click link below to view the profiler report", FileLink(profiler_report_name+"/profiler-output/profiler-report.html"))
   ```
**Tip**  
If the HTML reports do not render plots in the JupyterLab view, you must choose **Trust HTML** at the top of the reports\.  
To identify training issues, such as overfitting, vanishing gradients, and other problems that prevents your model from converging, use SageMaker Debugger and take automated actions while prototyping and training your ML models\. For more information, see [Amazon SageMaker Debugger](train-debugger.md)\. To find a complete analysis of model parameters, see the [Explainability with Amazon SageMaker Debugger](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker-debugger/xgboost_census_explanations/xgboost-census-debugger-rules.html#Explainability-with-Amazon-SageMaker-Debugger) example notebook\. 

You now have a trained XGBoost model\. SageMaker stores the model artifact in your S3 bucket\. To find the location of the model artifact, run the following code to print the model\_data attribute of the `xgb_model` estimator:

```
xgb_model.model_data
```

**Tip**  
To measure biases that can occur during each stage of the ML lifecycle \(data collection, model training and tuning, and monitoring of ML models deployed for prediction\), use SageMaker Clarify\. For more information, see [Amazon SageMaker Clarify Model Explainability](clarify-model-explainability.md)\. For an end\-to\-end example of it, see the [Fairness and Explainability with SageMaker Clarify](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_processing/fairness_and_explainability/fairness_and_explainability.html) example notebook\.