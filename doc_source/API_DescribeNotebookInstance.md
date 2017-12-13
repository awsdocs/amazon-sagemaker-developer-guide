# DescribeNotebookInstance<a name="API_DescribeNotebookInstance"></a>

Returns information about a notebook instance\.

## Request Syntax<a name="API_DescribeNotebookInstance_RequestSyntax"></a>

```
{
   "NotebookInstanceName": "string"
}
```

## Request Parameters<a name="API_DescribeNotebookInstance_RequestParameters"></a>

For information about the parameters that are common to all actions, see Common Parameters\.

The request accepts the following data in JSON format\.

 ** NotebookInstanceName **   
The name of the notebook instance that you want information about\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeNotebookInstance_ResponseSyntax"></a>

```
{
   "CreationTime": number,
   "FailureReason": "string",
   "InstanceType": "string",
   "KmsKeyId": "string",
   "LastModifiedTime": number,
   "NetworkInterfaceId": "string",
   "NotebookInstanceArn": "string",
   "NotebookInstanceName": "string",
   "NotebookInstanceStatus": "string",
   "RoleArn": "string",
   "SecurityGroups": [ "string" ],
   "SubnetId": "string",
   "Url": "string"
}
```

## Response Elements<a name="API_DescribeNotebookInstance_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** CreationTime **   
A timestamp\. Use this parameter to return the time when the notebook instance was created  
Type: Timestamp

 ** FailureReason **   
If staus is failed, the reason it failed\.  
Type: String  
Length Constraints: Maximum length of 1024\.

 ** InstanceType **   
The type of ML compute instance running on the notebook instance\.  
Type: String  
Valid Values:` ml.t2.medium | ml.m4.xlarge | ml.p2.xlarge` 

 ** KmsKeyId **   
 AWS KMS key ID Amazon SageMaker uses to encrypt data when storing it on the ML storage volume attached to the instance\.   
Type: String  
Length Constraints: Maximum length of 2048\.

 ** LastModifiedTime **   
A timestamp\. Use this parameter to retrieve the time when the notebook instance was last modified\.   
Type: Timestamp

 ** NetworkInterfaceId **   
 Network interface IDs that Amazon SageMaker created at the time of creating the instance\.   
Type: String

 ** NotebookInstanceArn **   
The Amazon Resource Name \(ARN\) of the notebook instance\.  
Type: String  
Length Constraints: Maximum length of 256\.

 ** NotebookInstanceName **   
 Name of the Amazon SageMaker notebook instance\.   
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** NotebookInstanceStatus **   
The status of the notebook instance\.  
Type: String  
Valid Values:` Pending | InService | Stopping | Stopped | Failed | Deleting` 

 ** RoleArn **   
 Amazon Resource Name \(ARN\) of the IAM role associated with the instance\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$` 

 ** SecurityGroups **   
The IDs of the VPC security groups\.  
Type: Array of strings  
Array Members: Maximum number of 5 items\.  
Length Constraints: Maximum length of 32\.

 ** SubnetId **   
The ID of the VPC subnet\.  
Type: String  
Length Constraints: Maximum length of 32\.

 ** Url **   
The URL that you use to connect to the Jupyter notebook that is running in your notebook instance\.   
Type: String

## Errors<a name="API_DescribeNotebookInstance_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DescribeNotebookInstance_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeNotebookInstance) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeNotebookInstance) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeNotebookInstance) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeNotebookInstance) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeNotebookInstance) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeNotebookInstance) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeNotebookInstance) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeNotebookInstance) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeNotebookInstance) 