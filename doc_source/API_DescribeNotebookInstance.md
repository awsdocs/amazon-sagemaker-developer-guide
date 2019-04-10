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
   "[AcceleratorTypes](#SageMaker-DescribeNotebookInstance-response-AcceleratorTypes)": [ "string" ],
   "[AdditionalCodeRepositories](#SageMaker-DescribeNotebookInstance-response-AdditionalCodeRepositories)": [ "string" ],
   "[CreationTime](#SageMaker-DescribeNotebookInstance-response-CreationTime)": number,
   "[DefaultCodeRepository](#SageMaker-DescribeNotebookInstance-response-DefaultCodeRepository)": "string",
   "[DirectInternetAccess](#SageMaker-DescribeNotebookInstance-response-DirectInternetAccess)": "string",
   "[FailureReason](#SageMaker-DescribeNotebookInstance-response-FailureReason)": "string",
   "[InstanceType](#SageMaker-DescribeNotebookInstance-response-InstanceType)": "string",
   "[KmsKeyId](#SageMaker-DescribeNotebookInstance-response-KmsKeyId)": "string",
   "[LastModifiedTime](#SageMaker-DescribeNotebookInstance-response-LastModifiedTime)": number,
   "[NetworkInterfaceId](#SageMaker-DescribeNotebookInstance-response-NetworkInterfaceId)": "string",
   "[NotebookInstanceArn](#SageMaker-DescribeNotebookInstance-response-NotebookInstanceArn)": "string",
   "[NotebookInstanceLifecycleConfigName](#SageMaker-DescribeNotebookInstance-response-NotebookInstanceLifecycleConfigName)": "string",
   "[NotebookInstanceName](#SageMaker-DescribeNotebookInstance-response-NotebookInstanceName)": "string",
   "[NotebookInstanceStatus](#SageMaker-DescribeNotebookInstance-response-NotebookInstanceStatus)": "string",
   "[RoleArn](#SageMaker-DescribeNotebookInstance-response-RoleArn)": "string",
   "[RootAccess](#SageMaker-DescribeNotebookInstance-response-RootAccess)": "string",
   "[SecurityGroups](#SageMaker-DescribeNotebookInstance-response-SecurityGroups)": [ "string" ],
   "[SubnetId](#SageMaker-DescribeNotebookInstance-response-SubnetId)": "string",
   "[Url](#SageMaker-DescribeNotebookInstance-response-Url)": "string",
   "[VolumeSizeInGB](#SageMaker-DescribeNotebookInstance-response-VolumeSizeInGB)": number
}
```

## Response Elements<a name="API_DescribeNotebookInstance_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [AcceleratorTypes](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-AcceleratorTypes"></a>
A list of the Elastic Inference \(EI\) instance types associated with this notebook instance\. Currently only one EI instance type can be associated with a notebook instance\. For more information, see [Using Elastic Inference in Amazon SageMaker](http://docs.aws.amazon.com/sagemaker/latest/dg/ei.html)\.  
Type: Array of strings  
Valid Values:` ml.eia1.medium | ml.eia1.large | ml.eia1.xlarge` 

 ** [AdditionalCodeRepositories](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-AdditionalCodeRepositories"></a>
An array of up to three Git repositories associated with the notebook instance\. These can be either the names of Git repositories stored as resources in your account, or the URL of Git repositories in [AWS CodeCommit](http://docs.aws.amazon.com/codecommit/latest/userguide/welcome.html) or in any other Git repository\. These repositories are cloned at the same level as the default repository of your notebook instance\. For more information, see [Associating Git Repositories with Amazon SageMaker Notebook Instances](http://docs.aws.amazon.com/sagemaker/latest/dg/nbi-git-repo.html)\.  
Type: Array of strings  
Array Members: Maximum number of 3 items\.  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `^https://([^/]+)/?(.*)$|^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [CreationTime](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-CreationTime"></a>
A timestamp\. Use this parameter to return the time when the notebook instance was created  
Type: Timestamp

 ** [DefaultCodeRepository](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-DefaultCodeRepository"></a>
The Git repository associated with the notebook instance as its default code repository\. This can be either the name of a Git repository stored as a resource in your account, or the URL of a Git repository in [AWS CodeCommit](http://docs.aws.amazon.com/codecommit/latest/userguide/welcome.html) or in any other Git repository\. When you open a notebook instance, it opens in the directory that contains this repository\. For more information, see [Associating Git Repositories with Amazon SageMaker Notebook Instances](http://docs.aws.amazon.com/sagemaker/latest/dg/nbi-git-repo.html)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `^https://([^/]+)/?(.*)$|^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [DirectInternetAccess](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-DirectInternetAccess"></a>
Describes whether Amazon SageMaker provides internet access to the notebook instance\. If this value is set to *Disabled*, the notebook instance does not have internet access, and cannot connect to Amazon SageMaker training and endpoint services\.  
For more information, see [Notebook Instances Are Internet\-Enabled by Default](https://docs.aws.amazon.com/sagemaker/latest/dg/appendix-additional-considerations.html#appendix-notebook-and-internet-access)\.  
Type: String  
Valid Values:` Enabled | Disabled` 

 ** [FailureReason](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-FailureReason"></a>
If status is `Failed`, the reason it failed\.  
Type: String  
Length Constraints: Maximum length of 1024\.

 ** [InstanceType](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-InstanceType"></a>
The type of ML compute instance running on the notebook instance\.  
Type: String  
Valid Values:` ml.t2.medium | ml.t2.large | ml.t2.xlarge | ml.t2.2xlarge | ml.t3.medium | ml.t3.large | ml.t3.xlarge | ml.t3.2xlarge | ml.m4.xlarge | ml.m4.2xlarge | ml.m4.4xlarge | ml.m4.10xlarge | ml.m4.16xlarge | ml.m5.xlarge | ml.m5.2xlarge | ml.m5.4xlarge | ml.m5.12xlarge | ml.m5.24xlarge | ml.c4.xlarge | ml.c4.2xlarge | ml.c4.4xlarge | ml.c4.8xlarge | ml.c5.xlarge | ml.c5.2xlarge | ml.c5.4xlarge | ml.c5.9xlarge | ml.c5.18xlarge | ml.c5d.xlarge | ml.c5d.2xlarge | ml.c5d.4xlarge | ml.c5d.9xlarge | ml.c5d.18xlarge | ml.p2.xlarge | ml.p2.8xlarge | ml.p2.16xlarge | ml.p3.2xlarge | ml.p3.8xlarge | ml.p3.16xlarge` 

 ** [KmsKeyId](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-KmsKeyId"></a>
The AWS KMS key ID Amazon SageMaker uses to encrypt data when storing it on the ML storage volume attached to the instance\.   
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `.*` 

 ** [LastModifiedTime](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-LastModifiedTime"></a>
A timestamp\. Use this parameter to retrieve the time when the notebook instance was last modified\.   
Type: Timestamp

 ** [NetworkInterfaceId](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-NetworkInterfaceId"></a>
The network interface IDs that Amazon SageMaker created at the time of creating the instance\.   
Type: String

 ** [NotebookInstanceArn](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-NotebookInstanceArn"></a>
The Amazon Resource Name \(ARN\) of the notebook instance\.  
Type: String  
Length Constraints: Maximum length of 256\.

 ** [NotebookInstanceLifecycleConfigName](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-NotebookInstanceLifecycleConfigName"></a>
Returns the name of a notebook instance lifecycle configuration\.  
For information about notebook instance lifestyle configurations, see [Step 2\.1: \(Optional\) Customize a Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/notebook-lifecycle-config.html)   
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [NotebookInstanceName](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-NotebookInstanceName"></a>
The name of the Amazon SageMaker notebook instance\.   
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [NotebookInstanceStatus](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-NotebookInstanceStatus"></a>
The status of the notebook instance\.  
Type: String  
Valid Values:` Pending | InService | Stopping | Stopped | Failed | Deleting | Updating` 

 ** [RoleArn](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-RoleArn"></a>
The Amazon Resource Name \(ARN\) of the IAM role associated with the instance\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$` 

 ** [RootAccess](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-RootAccess"></a>
Whether root access is enabled or disabled for users of the notebook instance\.  
Lifecycle configurations need root access to be able to set up a notebook instance\. Because of this, lifecycle configurations associated with a notebook instance always run with root access even if you disable root access for users\.
Type: String  
Valid Values:` Enabled | Disabled` 

 ** [SecurityGroups](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-SecurityGroups"></a>
The IDs of the VPC security groups\.  
Type: Array of strings  
Array Members: Maximum number of 5 items\.  
Length Constraints: Maximum length of 32\.  
Pattern: `[-0-9a-zA-Z]+` 

 ** [SubnetId](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-SubnetId"></a>
The ID of the VPC subnet\.  
Type: String  
Length Constraints: Maximum length of 32\.  
Pattern: `[-0-9a-zA-Z]+` 

 ** [Url](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-Url"></a>
The URL that you use to connect to the Jupyter notebook that is running in your notebook instance\.   
Type: String

 ** [VolumeSizeInGB](#API_DescribeNotebookInstance_ResponseSyntax) **   <a name="SageMaker-DescribeNotebookInstance-response-VolumeSizeInGB"></a>
The size, in GB, of the ML storage volume attached to the notebook instance\.  
Type: Integer  
Valid Range: Minimum value of 5\. Maximum value of 16384\.

## Errors<a name="API_DescribeNotebookInstance_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DescribeNotebookInstance_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeNotebookInstance) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeNotebookInstance) 