# CreateTrainingJob<a name="API_CreateTrainingJob"></a>

 Starts a model training job\. After training completes, Amazon SageMaker saves the resulting model artifacts to an Amazon S3 location that you specify\. 

If you choose to host your model using Amazon SageMaker hosting services, you can use the resulting model artifacts as part of the model\. You can also use the artifacts in a deep learning service other than Amazon SageMaker, provided that you know how to use them for inferences\. 

In the request body, you provide the following: 

+  `AlgorithmSpecification` \- Identifies the training algorithm to use\. 

+  `HyperParameters` \- Specify these algorithm\-specific parameters to influence the quality of the final model\. For a list of hyperparameters for each training algorithm provided by Amazon SageMaker, see [Algorithms](http://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\. 

+  `InputDataConfig` \- Describes the training dataset and the Amazon S3 location where it is stored\.

+  `OutputDataConfig` \- Identifies the Amazon S3 location where you want Amazon SageMaker to save the results of model training\. 

  

+  `ResourceConfig` \- Identifies the resources, ML compute instances, and ML storage volumes to deploy for model training\. In distributed training, you specify more than one instance\. 

+  `RoleARN` \- The Amazon Resource Number \(ARN\) that Amazon SageMaker assumes to perform tasks on your behalf during model training\. You must grant this role the necessary permissions so that Amazon SageMaker can successfully complete model training\. 

+  `StoppingCondition` \- Sets a duration for training\. Use this parameter to cap model training costs\. 

 For more information about Amazon SageMaker, see [How It Works](http://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works.html)\. 

## Request Syntax<a name="API_CreateTrainingJob_RequestSyntax"></a>

```
{
   "AlgorithmSpecification": { 
      "TrainingImage": "string",
      "TrainingInputMode": "string"
   },
   "HyperParameters": { 
      "string" : "string" 
   },
   "InputDataConfig": [ 
      { 
         "ChannelName": "string",
         "CompressionType": "string",
         "ContentType": "string",
         "DataSource": { 
            "S3DataSource": { 
               "S3DataDistributionType": "string",
               "S3DataType": "string",
               "S3Uri": "string"
            }
         },
         "RecordWrapperType": "string"
      }
   ],
   "OutputDataConfig": { 
      "KmsKeyId": "string",
      "S3OutputPath": "string"
   },
   "ResourceConfig": { 
      "InstanceCount": number,
      "InstanceType": "string",
      "VolumeKmsKeyId": "string",
      "VolumeSizeInGB": number
   },
   "RoleArn": "string",
   "StoppingCondition": { 
      "MaxRuntimeInSeconds": number
   },
   "Tags": [ 
      { 
         "Key": "string",
         "Value": "string"
      }
   ],
   "TrainingJobName": "string"
}
```

## Request Parameters<a name="API_CreateTrainingJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see Common Parameters\.

The request accepts the following data in JSON format\.

 ** AlgorithmSpecification **   
The registry path of the Docker image that contains the training algorithm and algorithm\-specific metadata, including the input mode\. For more information about algorithms provided by Amazon SageMaker, see [Algorithms](http://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\. For information about providing your own algorithms, see [Bring Your Own Algorithms ](http://docs.aws.amazon.com/sagemaker/latest/dg/adv-topics-own-algo.html)\.   
Type: [AlgorithmSpecification](API_AlgorithmSpecification.md) object  
Required: Yes

 ** HyperParameters **   
Algorithm\-specific parameters\. You set hyperparameters before you start the learning process\. Hyperparameters influence the quality of the model\. For a list of hyperparameters for each training algorithm provided by Amazon SageMaker, see [Algorithms](http://docs.aws.amazon.com/sagemaker/latest/dg/algos.html)\.   
You can specify a maximum of 100 hyperparameters\. Each hyperparameter is a key\-value pair\. Each key and value is limited to 256 characters, as specified by the `Length Constraint`\.   
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Value Length Constraints: Maximum length of 256\.  
Required: No

 ** InputDataConfig **   
An array of `Channel` objects\. Each channel is a named input source\. `InputDataConfig` describes the input data and its location\.   
Algorithms can accept input data from one or more channels\. For example, an algorithm might have two channels of input data, `training_data` and `validation_data`\. The configuration for each channel provides the S3 location where the input data is stored\. It also provides information about the stored data: the MIME type, compression method, and whether the data is wrapped in RecordIO format\.   
Depending on the input mode that the algorithm supports, Amazon SageMaker either copies input data files from an S3 bucket to a local directory in the Docker container, or makes it available as input streams\.   
Type: Array of [Channel](API_Channel.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 8 items\.  
Required: Yes

 ** OutputDataConfig **   
Specifies the path to the S3 bucket where you want to store model artifacts\. Amazon SageMaker creates subfolders for the artifacts\.   
Type: [OutputDataConfig](API_OutputDataConfig.md) object  
Required: Yes

 ** ResourceConfig **   
The resources, including the ML compute instances and ML storage volumes, to use for model training\.   
ML storage volumes store model artifacts and incremental states\. Training algorithms might also use ML storage volumes for scratch space\. If you want Amazon SageMaker to use the ML storage volume to store the training data, choose `File` as the `TrainingInputMode` in the algorithm specification\. For distributed training algorithms, specify an instance count greater than 1\.  
Type: [ResourceConfig](API_ResourceConfig.md) object  
Required: Yes

 ** RoleArn **   
The Amazon Resource Name \(ARN\) of an IAM role that Amazon SageMaker can assume to perform tasks on your behalf\.   
During model training, Amazon SageMaker needs your permission to read input data from an S3 bucket, download a Docker image that contains training code, write model artifacts to an S3 bucket, write logs to Amazon CloudWatch Logs, and publish metrics to Amazon CloudWatch\. You grant permissions for all of these tasks to an IAM role\. For more information, see [Amazon SageMaker Roles](http://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: Yes

 ** StoppingCondition **   
Sets a duration for training\. Use this parameter to cap model training costs\. To stop a job, Amazon SageMaker sends the algorithm the `SIGTERM` signal, which delays job termination for 120 seconds\. Algorithms might use this 120\-second window to save the model artifacts\.   
When Amazon SageMaker terminates a job because the stopping condition has been met, training algorithms provided by Amazon SageMaker save the intermediate results of the job\. This intermediate data is a valid model artifact\. You can use it to create a model using the `CreateModel` API\.   
Type: [StoppingCondition](API_StoppingCondition.md) object  
Required: Yes

 ** Tags **   
An array of key\-value pairs\. For more information, see [Using Cost Allocation Tags](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\.   
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

 ** TrainingJobName **   
The name of the training job\. The name must be unique within an AWS Region in an AWS account\. It appears in the Amazon SageMaker console\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_CreateTrainingJob_ResponseSyntax"></a>

```
{
   "TrainingJobArn": "string"
}
```

## Response Elements<a name="API_CreateTrainingJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** TrainingJobArn **   
The Amazon Resource Name \(ARN\) of the training job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws:sagemaker:[\p{Alnum}\-]*:[0-9]{12}:training-job/.*` 

## Errors<a name="API_CreateTrainingJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceInUse**   
Resource being accessed is in use\.  
HTTP Status Code: 400

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_CreateTrainingJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateTrainingJob) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateTrainingJob) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateTrainingJob) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateTrainingJob) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateTrainingJob) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateTrainingJob) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateTrainingJob) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateTrainingJob) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateTrainingJob) 