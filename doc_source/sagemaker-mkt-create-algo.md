# Create an Algorithm Resource<a name="sagemaker-mkt-create-algo"></a>

To create an algorithm resource that you can use to run training jobs in Amazon SageMaker and publish on AWS Marketplace specify the following information:
+ The Docker containers that contains the training and, optionally, inference code\.
+ The configuration of the input data that your algorithm expects for training\.
+ The hyperparameters that your algorithm supports\.
+ Metrics that your algorithm sends to Amazon CloudWatch during training jobs\.
+ The instance types that your algorithm supports for training and inference, and whether it supports distributed training across multiple instances\.
+ Validation profiles, which are training jobs that SageMaker uses to test your algorithm's training code and batch transform jobs that SageMaker runs to test your algorithm's inference code\.

  To ensure that buyers and sellers can be confident that products work in SageMaker, we require that you validate your algorithms before listing them on AWS Marketplace\. You can list products in the AWS Marketplace only if validation succeeds\. To validate your algorithms, SageMaker uses your validation profile and sample data to run the following validations tasks:

  1. Create a training job in your account to verify that your training image works with SageMaker\.

  1. If you included inference code in your algorithm, create a model in your account using the algorithm's inference image and the model artifacts produced by the training job\.

  1. If you included inference code in your algorithm, create a transform job in your account using the model to verify that your inference image works with SageMaker\.

  When you list your product on AWS Marketplace, the inputs and outputs of this validation process persist as part of your product and are made available to your buyers\. This helps buyers understand and evaluate the product before they buy it\. For example, buyers can inspect the input data that you used, the outputs generated, and the logs and metrics emitted by your code\. The more comprehensive your validation specification, the easier it is for customers to evaluate your product\.
**Note**  
In your validation profile, provide only data that you want to expose publicly\.

  Validation can take up to a few hours\. To see the status of the jobs in your account, in the SageMaker console, see the **Training jobs** and **Transform jobs** pages\. If validation fails, you can access the scan and validation reports from the SageMaker console\. If any issues are found, you will have to create the algorithm again\.
**Note**  
To publish your algorithm on AWS Marketplace, at least one validation profile is required\.

You can create an algorithm by using either the SageMaker console or the SageMaker API\.

**Topics**
+ [Create an Algorithm Resource \(Console\)](#sagemaker-mkt-create-algo-console)
+ [Create an Algorithm Resource \(API\)](#sagemaker-mkt-create-algo-api)

## Create an Algorithm Resource \(Console\)<a name="sagemaker-mkt-create-algo-console"></a>

**To create an algorithm resource \(console\)**

1. Open the SageMaker console at [https://console\.aws\.amazon\.com/sagemaker/](https://console.aws.amazon.com/sagemaker/)\.

1. From the left menu, choose **Training**\.

1. From the dropdown menu, choose **Algorithms**, then choose **Create algorithm**\.

1. On the **Training specifications** page, provide the following information:

   1. For **Algorithm name**, type a name for your algorithm\. The algorithm name must be unique in your account and in the AWS region\. The name must have 1 to 64 characters\. Valid characters are a\-z, A\-Z, 0\-9, and \- \(hyphen\)\.

   1. Type a description for your algorithm\. This description appears in the SageMaker console and in the AWS Marketplace\.

   1. For **Training image, type the path in Amazon ECR where your training container is stored\.**

   1. For **Support distributed training**, Choose **Yes** if your algorithm supports training on multiple instances\. Otherwise, choose **No**\.

   1. For **Support instance types for training**, choose the instance types that your algorithm supports\.

   1. For **Channel specification**, specify up to 8 channels of input data for your algorithm\. For example, you might specify 3 input channels named `train`, `validation`, and `test`\. For each channel, specify the following information:

      1. For **Channel name**, type a name for the channel\. The name must have 1 to 64 characters\. Valid characters are a\-z, A\-Z, 0\-9, and \- \(hyphen\)\.

      1. To require the channel for your algorithm, choose **Channel required**\.

      1. Type a description for the channel\.

      1. For **Supported input modes**, choose **Pipe mode** if your algorithm supports streaming the input data, and **File mode** if your algorithm supports downloading the input data as a file\. You can choose both\.

      1. For **Supported content types**, type the MIME type that your algorithm expects for input data\.

      1. For **Supported compression type**, choose **Gzip** if your algorithm supports Gzip compression\. Otherwise, choose **None**\.

      1. Choose **Add channel** to add another data input channel, or choose **Next** if you are done adding channels\.

1. On the **Tuning specifications** page, provide the following information:

   1. For **Hyperparameter specification**, specify the hyperparameters that your algorithm supports by editing the JSON object\. For each hyperparameter that your algorithm supports, construct a JSON block similar to the following:

      ```
      {
      "DefaultValue": "5",
      "Description": "The first hyperparameter",
      "IsRequired": true,
      "IsTunable": false,
      "Name": "intRange",
      "Range": {
      "IntegerParameterRangeSpecification": {
      "MaxValue": "10",
      "MinValue": "1"
      },
      "Type": "Integer"
      }
      ```

      In the JSON, supply the following:

      1. For `DefaultValue`, specify a default value for the hyperparameter, if there is one\.

      1. For `Description`, specify a description for the hyperparameter\.

      1. For `IsRequired`, specify whether the hyperparameter is required\.

      1. For `IsTunable`, specify `true` if this hyperparameter can be tuned when a user runs a hyperparameter tuning job that uses this algorithm\. For information, see [Perform Automatic Model Tuning with SageMaker](automatic-model-tuning.md)\.

      1. For `Name`, specify a name for the hyperparameter\.

      1. For `Range`, specify one of the following:
         + `IntegerParameterRangeSpecification` \- the values of the hyperparameter are integers\. Specify minimum and maximum values for the hyperparameter\.
         + 
         + `ContinuousParameterRangeSpecification` \- the values of the hyperparameter are floating\-point values\. Specify minimum and maximum values for the hyperparameter\.
         + `CategoricalParameterRangeSpecification` \- the values of the hyperparameter are categorical values\. Specify a list of all of the possible values\.

      1. For `Type`, specify `Integer`, `Continuous`, or `Categorical`\. The value must correspond to the type of `Range` that you specified\.

   1. For **Metric definitions**, specify any training metrics that you want your algorithm to emit\. SageMaker uses the regular expression that you specify to find the metrics by parsing the logs from your training container during training\. Users can view these metrics when they run training jobs with your algorithm, and they can monitor and plot the metrics in Amazon CloudWatch\. For information, see [Monitor and Analyze Training Jobs Using Amazon CloudWatch Metrics](training-metrics.md)\. For each metric, provide the following information:

      1. For **Metric name**, type a name for the metric\.

      1. For `Regex`, type the regular expression that SageMaker uses to parse training logs so that it can find the metric value\.

      1. For **Objective metric support** choose **Yes** if this metric can be used as the objective metric for a hyperparameter tuning job\. For information, see [Perform Automatic Model Tuning with SageMaker](automatic-model-tuning.md)\.

      1. Choose **Add metric** to add another metric, or choose **Next** if you are done adding metrics\.

1. On the **Inference specifications** page, provide the following information if your algorithm supports inference:

   1. For **Location of inference image**, type the path in Amazon ECR where your inference container is stored\.

   1. For **Container DNS host name**, type the name of a DNS host for your image\.

   1. For **Supported instance types for real\-time inference**, choose the instance types that your algorithm supports for models deployed as hosted endpoints in SageMaker\. For information, see [Deploy Models for Inference](deploy-model.md)\.

   1. For **Supported instance types for batch transform jobs**, choose the instance types that your algorithm supports for batch transform jobs\. For information, see [Use Batch Transform](batch-transform.md)\.

   1. For **Supported content types**, type the type of input data that your algorithm expects for inference requests\.

   1. For **Supported response MIME types**, type the MIME types that your algorithm supports for inference responses\.

   1. Choose **Next**\.

1. On the **Validation specifications** page, provide the following information:

   1. For **Publish this algorithm on AWS Marketplace**, choose **Yes** to publish the algorithm on AWS Marketplace\.

   1. For **Validate this resource**, choose **Yes** if you want SageMaker to run training jobs and/or batch transform jobs that you specify to test the training and/or inference code of your algorithm\.
**Note**  
To publish your algorithm on AWS Marketplace, your algorithm must be validated\.

   1. For **IAM role**, choose an IAM role that has the required permissions to run training jobs and batch transform jobs in SageMaker, or choose **Create a new role** to allow SageMaker to create a role that has the `AmazonSageMakerFullAccess` managed policy attached\. For information, see [SageMaker Roles](sagemaker-roles.md)\.

   1. For **Validation profile**, specify the following:
      + A name for the validation profile\.
      + A **Training job definition**\. This is a JSON block that describes a training job\. This is in the same format as the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TrainingJobDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TrainingJobDefinition.html) input parameter of the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAlgorithm.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAlgorithm.html) API\.
      + A **Transform job definition**\. This is a JSON block that describes a batch transform job\. This is in the same format as the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TransformJobDefinition.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_TransformJobDefinition.html) input parameter of the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAlgorithm.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAlgorithm.html) API\.

   1. Choose **Create algorithm**\.

## Create an Algorithm Resource \(API\)<a name="sagemaker-mkt-create-algo-api"></a>

To create an algorithm resource by using the SageMaker API, call the [https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAlgorithm.html](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_CreateAlgorithm.html) API\. 