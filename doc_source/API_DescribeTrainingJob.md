# DescribeTrainingJob<a name="API_DescribeTrainingJob"></a>

Returns information about a training job\.

## Request Syntax<a name="API_DescribeTrainingJob_RequestSyntax"></a>

```
{
   "TrainingJobName": "string"
}
```

## Request Parameters<a name="API_DescribeTrainingJob_RequestParameters"></a>

For information about the parameters that are common to all actions, see Common Parameters\.

The request accepts the following data in JSON format\.

 ** TrainingJobName **   
The name of the training job\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeTrainingJob_ResponseSyntax"></a>

```
{
   "AlgorithmSpecification": { 
      "TrainingImage": "string",
      "TrainingInputMode": "string"
   },
   "CreationTime": number,
   "FailureReason": "string",
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
   "LastModifiedTime": number,
   "ModelArtifacts": { 
      "S3ModelArtifacts": "string"
   },
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
   "SecondaryStatus": "string",
   "StoppingCondition": { 
      "MaxRuntimeInSeconds": number
   },
   "TrainingEndTime": number,
   "TrainingJobArn": "string",
   "TrainingJobName": "string",
   "TrainingJobStatus": "string",
   "TrainingStartTime": number
}
```

## Response Elements<a name="API_DescribeTrainingJob_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** AlgorithmSpecification **   
Information about the algorithm used for training, and algorithm metadata\.   
Type: [AlgorithmSpecification](API_AlgorithmSpecification.md) object

 ** CreationTime **   
A timestamp that indicates when the training job was created\.  
Type: Timestamp

 ** FailureReason **   
If the training job failed, the reason it failed\.   
Type: String  
Length Constraints: Maximum length of 1024\.

 ** HyperParameters **   
Algorithm\-specific parameters\.   
Type: String to string map  
Key Length Constraints: Maximum length of 256\.  
Value Length Constraints: Maximum length of 256\.

 ** InputDataConfig **   
An array of `Channel` objects that describes each data input channel\.   
Type: Array of [Channel](API_Channel.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 8 items\.

 ** LastModifiedTime **   
A timestamp that indicates when the status of the training job was last modified\.  
Type: Timestamp

 ** ModelArtifacts **   
Information about the Amazon S3 location that is configured for storing model artifacts\.   
Type: [ModelArtifacts](API_ModelArtifacts.md) object

 ** OutputDataConfig **   
The S3 path where model artifacts that you configured when creating the job are stored\. Amazon SageMaker creates subfolders for model artifacts\.   
Type: [OutputDataConfig](API_OutputDataConfig.md) object

 ** ResourceConfig **   
Resources, including ML compute instances and ML storage volumes, that are configured for model training\.   
Type: [ResourceConfig](API_ResourceConfig.md) object

 ** RoleArn **   
The AWS Identity and Access Management \(IAM\) role configured for the training job\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$` 

 ** SecondaryStatus **   
 Provides granular information about the system state\. For more information, see `TrainingJobStatus`\.   
Type: String  
Valid Values:` Starting | Downloading | Training | Uploading | Stopping | Stopped | MaxRuntimeExceeded | Completed | Failed` 

 ** StoppingCondition **   
The condition under which to stop the training job\.   
Type: [StoppingCondition](API_StoppingCondition.md) object

 ** TrainingEndTime **   
A timestamp that indicates when model training ended\.  
Type: Timestamp

 ** TrainingJobArn **   
The Amazon Resource Name \(ARN\) of the training job\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws:sagemaker:[\p{Alnum}\-]*:[0-9]{12}:training-job/.*` 

 ** TrainingJobName **   
 Name of the model training job\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** TrainingJobStatus **   
The status of the training job\.   
For the `InProgress` status, Amazon SageMaker can return these secondary statuses:  

+ Starting \- Preparing for training\.

+ Downloading \- Optional stage for algorithms that support File training input mode\. It indicates data is being downloaded to ML storage volumes\.

+ Training \- Training is in progress\.

+ Uploading \- Training is complete and model upload is in progress\.
For the `Stopped` training status, Amazon SageMaker can return these secondary statuses:  

+ MaxRuntimeExceeded \- Job stopped as a result of maximum allowed runtime exceeded\.
Type: String  
Valid Values:` InProgress | Completed | Failed | Stopping | Stopped` 

 ** TrainingStartTime **   
A timestamp that indicates when training started\.  
Type: Timestamp

## Errors<a name="API_DescribeTrainingJob_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceNotFound**   
Resource being access is not found\.  
HTTP Status Code: 400

## See Also<a name="API_DescribeTrainingJob_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeTrainingJob) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeTrainingJob) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeTrainingJob) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeTrainingJob) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeTrainingJob) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeTrainingJob) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeTrainingJob) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeTrainingJob) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeTrainingJob) 