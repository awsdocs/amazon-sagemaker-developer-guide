# DescribeNotebookInstance<a name="API_DescribeNotebookInstance"></a>

Returns information about a notebook instance\.

## Request Syntax<a name="API_DescribeNotebookInstance_RequestSyntax"></a>

```
{
   "[NotebookInstanceName](#SageMaker-DescribeNotebookInstance-request-NotebookInstanceName)": "string"
}
```

## Request Parameters<a name="API_DescribeNotebookInstance_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [NotebookInstanceName](#API_DescribeNotebookInstance_RequestSyntax) **   <a name="SageMaker-DescribeNotebookInstance-request-NotebookInstanceName"></a>
The name of the notebook instance that you want information about\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeNotebookInstance_ResponseSyntax"></a>

```
{
   "[CreationTime](#SageMaker-DescribeNotebookInstance-response-CreationTime)": number,
   "[DirectInternetAccess](#SageMaker-DescribeNotebookInstance-response-DirectInternetAccess)": "string",
   "[FailureReason](#SageMaker-DescribeNotebookInstance-response-FailureReason)": "string",
   "[InstanceType](#SageMaker-DescribeNotebookInstance-response-InstanceType)": "string",
   "[KmsKeyId](#SageMaker-DescribeNotebookInstance-response-KmsKeyId)": "string",
   "[LastModifiedTime](#SageMaker-DescribeNotebookInstance-response-LastModifiedTime)": number,
   "[NetworkInterfaceId](#SageMaker-DescribeNotebookInstance-response-NetworkInterfaceId)": "string",
   "[NotebookInstanceArn](#SageMaker-DescribeNotebookInstance-response-NotebookInstanceArn)": "string",
   "[NotebookInstanceName](#SageMaker-DescribeNotebookInstance-response-NotebookInstanceName)": "string",
   "[NotebookInstanceStatus](#SageMaker-DescribeNotebookInstance-response-NotebookInstanceStatus)": "string",
   "[RoleArn](#SageMaker-DescribeNotebookInstance-response-RoleArn)": "string",
   "[SecurityGroups](#SageMaker-DescribeNotebookInstance-response-SecurityGroups)": [ "string" ],
   "[SubnetId](#SageMaker-DescribeNotebookInstance-response-SubnetId)": "string",
   "[Url](#SageMaker-DescribeNotebookInstance-response-Url)": "string"
}
```

## Response Elements<a name="API_DescribeNotebookInstance_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [CreationTime](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-CreationTime"></a>
A timestamp\. Use this parameter to return the time when the notebook instance was created  
Type: Timestamp

 ** [DirectInternetAccess](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-DirectInternetAccess"></a>
Describes whether Amazon SageMaker provides internet access to the notebook instance\. If this value is set to *Disabled, he notebook instance does not have internet access, and cannot connect to Amazon SageMaker training and endpoint services*\.  
For more information, see [Notebook Instances Are Internet\-Enabled by Default](appendix-additional-considerations.md#appendix-notebook-and-internet-access)\.  
Type: String  
Valid Values:` Enabled | Disabled` 

 ** [FailureReason](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-FailureReason"></a>
If status is failed, the reason it failed\.  
Type: String  
Length Constraints: Maximum length of 1024\.

 ** [InstanceType](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-InstanceType"></a>
The type of ML compute instance running on the notebook instance\.  
Type: String  
Valid Values:` ml.t2.medium | ml.t2.large | ml.t2.xlarge | ml.t2.2xlarge | ml.m4.xlarge | ml.m4.2xlarge | ml.m4.4xlarge | ml.m4.10xlarge | ml.m4.16xlarge | ml.p2.xlarge | ml.p2.8xlarge | ml.p2.16xlarge | ml.p3.2xlarge | ml.p3.8xlarge | ml.p3.16xlarge` 

 ** [KmsKeyId](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-KmsKeyId"></a>
 AWS KMS key ID Amazon SageMaker uses to encrypt data when storing it on the ML storage volume attached to the instance\.   
Type: String  
Length Constraints: Maximum length of 2048\.

 ** [LastModifiedTime](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-LastModifiedTime"></a>
A timestamp\. Use this parameter to retrieve the time when the notebook instance was last modified\.   
Type: Timestamp

 ** [NetworkInterfaceId](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-NetworkInterfaceId"></a>
 Network interface IDs that Amazon SageMaker created at the time of creating the instance\.   
Type: String

 ** [NotebookInstanceArn](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-NotebookInstanceArn"></a>
The Amazon Resource Name \(ARN\) of the notebook instance\.  
Type: String  
Length Constraints: Maximum length of 256\.

 ** [NotebookInstanceName](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-NotebookInstanceName"></a>
 Name of the Amazon SageMaker notebook instance\.   
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [NotebookInstanceStatus](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-NotebookInstanceStatus"></a>
The status of the notebook instance\.  
Type: String  
Valid Values:` Pending | InService | Stopping | Stopped | Failed | Deleting | Updating` 

 ** [RoleArn](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-RoleArn"></a>
 Amazon Resource Name \(ARN\) of the IAM role associated with the instance\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$` 

 ** [SecurityGroups](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-SecurityGroups"></a>
The IDs of the VPC security groups\.  
Type: Array of strings  
Array Members: Maximum number of 5 items\.  
Length Constraints: Maximum length of 32\.

 ** [SubnetId](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-SubnetId"></a>
The ID of the VPC subnet\.  
Type: String  
Length Constraints: Maximum length of 32\.

 ** [Url](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-Url"></a>
The URL that you use to connect to the Jupyter notebook that is running in your notebook instance\.   
Type: String

## Errors<a name="API_DescribeNotebookInstance_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DescribeNotebookInstance_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeNotebookInstance) 