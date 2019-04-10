# UpdateNotebookInstance<a name="API_UpdateNotebookInstance"></a>

Updates a notebook instance\. NotebookInstance updates include upgrading or downgrading the ML compute instance used for your notebook instance to accommodate changes in your workload requirements\.

## Request Syntax<a name="API_UpdateNotebookInstance_RequestSyntax"></a>

```
{
   "[AcceleratorTypes](#SageMaker-UpdateNotebookInstance-request-AcceleratorTypes)": [ "string" ],
   "[AdditionalCodeRepositories](#SageMaker-UpdateNotebookInstance-request-AdditionalCodeRepositories)": [ "string" ],
   "[DefaultCodeRepository](#SageMaker-UpdateNotebookInstance-request-DefaultCodeRepository)": "string",
   "[DisassociateAcceleratorTypes](#SageMaker-UpdateNotebookInstance-request-DisassociateAcceleratorTypes)": boolean,
   "[DisassociateAdditionalCodeRepositories](#SageMaker-UpdateNotebookInstance-request-DisassociateAdditionalCodeRepositories)": boolean,
   "[DisassociateDefaultCodeRepository](#SageMaker-UpdateNotebookInstance-request-DisassociateDefaultCodeRepository)": boolean,
   "[DisassociateLifecycleConfig](#SageMaker-UpdateNotebookInstance-request-DisassociateLifecycleConfig)": boolean,
   "[InstanceType](#SageMaker-UpdateNotebookInstance-request-InstanceType)": "string",
   "[LifecycleConfigName](#SageMaker-UpdateNotebookInstance-request-LifecycleConfigName)": "string",
   "[NotebookInstanceName](#SageMaker-UpdateNotebookInstance-request-NotebookInstanceName)": "string",
   "[RoleArn](#SageMaker-UpdateNotebookInstance-request-RoleArn)": "string",
   "[RootAccess](#SageMaker-UpdateNotebookInstance-request-RootAccess)": "string",
   "[VolumeSizeInGB](#SageMaker-UpdateNotebookInstance-request-VolumeSizeInGB)": number
}
```

## Request Parameters<a name="API_UpdateNotebookInstance_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [AcceleratorTypes](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-AcceleratorTypes"></a>
A list of the Elastic Inference \(EI\) instance types to associate with this notebook instance\. Currently only one EI instance type can be associated with a notebook instance\. For more information, see [Using Elastic Inference in Amazon SageMaker](http://docs.aws.amazon.com/sagemaker/latest/dg/ei.html)\.  
Type: Array of strings  
Valid Values:` ml.eia1.medium | ml.eia1.large | ml.eia1.xlarge`   
Required: No

 ** [AdditionalCodeRepositories](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-AdditionalCodeRepositories"></a>
An array of up to three Git repositories to associate with the notebook instance\. These can be either the names of Git repositories stored as resources in your account, or the URL of Git repositories in [AWS CodeCommit](http://docs.aws.amazon.com/codecommit/latest/userguide/welcome.html) or in any other Git repository\. These repositories are cloned at the same level as the default repository of your notebook instance\. For more information, see [Associating Git Repositories with Amazon SageMaker Notebook Instances](http://docs.aws.amazon.com/sagemaker/latest/dg/nbi-git-repo.html)\.  
Type: Array of strings  
Array Members: Maximum number of 3 items\.  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `^https://([^/]+)/?(.*)$|^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 ** [DefaultCodeRepository](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-DefaultCodeRepository"></a>
The Git repository to associate with the notebook instance as its default code repository\. This can be either the name of a Git repository stored as a resource in your account, or the URL of a Git repository in [AWS CodeCommit](http://docs.aws.amazon.com/codecommit/latest/userguide/welcome.html) or in any other Git repository\. When you open a notebook instance, it opens in the directory that contains this repository\. For more information, see [Associating Git Repositories with Amazon SageMaker Notebook Instances](http://docs.aws.amazon.com/sagemaker/latest/dg/nbi-git-repo.html)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `^https://([^/]+)/?(.*)$|^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 ** [DisassociateAcceleratorTypes](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-DisassociateAcceleratorTypes"></a>
A list of the Elastic Inference \(EI\) instance types to remove from this notebook instance\. This operation is idempotent\. If you specify an accelerator type that is not associated with the notebook instance when you call this method, it does not throw an error\.  
Type: Boolean  
Required: No

 ** [DisassociateAdditionalCodeRepositories](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-DisassociateAdditionalCodeRepositories"></a>
A list of names or URLs of the default Git repositories to remove from this notebook instance\. This operation is idempotent\. If you specify a Git repository that is not associated with the notebook instance when you call this method, it does not throw an error\.  
Type: Boolean  
Required: No

 ** [DisassociateDefaultCodeRepository](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-DisassociateDefaultCodeRepository"></a>
The name or URL of the default Git repository to remove from this notebook instance\. This operation is idempotent\. If you specify a Git repository that is not associated with the notebook instance when you call this method, it does not throw an error\.  
Type: Boolean  
Required: No

 ** [DisassociateLifecycleConfig](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-DisassociateLifecycleConfig"></a>
Set to `true` to remove the notebook instance lifecycle configuration currently associated with the notebook instance\. This operation is idempotent\. If you specify a lifecycle configuration that is not associated with the notebook instance when you call this method, it does not throw an error\.  
Type: Boolean  
Required: No

 ** [InstanceType](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-InstanceType"></a>
The Amazon ML compute instance type\.  
Type: String  
Valid Values:` ml.t2.medium | ml.t2.large | ml.t2.xlarge | ml.t2.2xlarge | ml.t3.medium | ml.t3.large | ml.t3.xlarge | ml.t3.2xlarge | ml.m4.xlarge | ml.m4.2xlarge | ml.m4.4xlarge | ml.m4.10xlarge | ml.m4.16xlarge | ml.m5.xlarge | ml.m5.2xlarge | ml.m5.4xlarge | ml.m5.12xlarge | ml.m5.24xlarge | ml.c4.xlarge | ml.c4.2xlarge | ml.c4.4xlarge | ml.c4.8xlarge | ml.c5.xlarge | ml.c5.2xlarge | ml.c5.4xlarge | ml.c5.9xlarge | ml.c5.18xlarge | ml.c5d.xlarge | ml.c5d.2xlarge | ml.c5d.4xlarge | ml.c5d.9xlarge | ml.c5d.18xlarge | ml.p2.xlarge | ml.p2.8xlarge | ml.p2.16xlarge | ml.p3.2xlarge | ml.p3.8xlarge | ml.p3.16xlarge`   
Required: No

 ** [LifecycleConfigName](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-LifecycleConfigName"></a>
The name of a lifecycle configuration to associate with the notebook instance\. For information about lifestyle configurations, see [Step 2\.1: \(Optional\) Customize a Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/notebook-lifecycle-config.html)\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 ** [NotebookInstanceName](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-NotebookInstanceName"></a>
The name of the notebook instance to update\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [RoleArn](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-RoleArn"></a>
The Amazon Resource Name \(ARN\) of the IAM role that Amazon SageMaker can assume to access the notebook instance\. For more information, see [Amazon SageMaker Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)\.   
To be able to pass this role to Amazon SageMaker, the caller of this API must have the `iam:PassRole` permission\.
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: No

 ** [RootAccess](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-RootAccess"></a>
Whether root access is enabled or disabled for users of the notebook instance\. The default value is `Enabled`\.  
If you set this to `Disabled`, users don't have root access on the notebook instance, but lifecycle configuration scripts still run with root permissions\.
Type: String  
Valid Values:` Enabled | Disabled`   
Required: No

 ** [VolumeSizeInGB](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-VolumeSizeInGB"></a>
The size, in GB, of the ML storage volume to attach to the notebook instance\. The default value is 5 GB\.  
Type: Integer  
Valid Range: Minimum value of 5\. Maximum value of 16384\.  
Required: No

## Response Elements<a name="API_UpdateNotebookInstance_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_UpdateNotebookInstance_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_UpdateNotebookInstance_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/UpdateNotebookInstance) 