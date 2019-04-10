# CreateNotebookInstance<a name="API_CreateNotebookInstance"></a>

Creates an Amazon SageMaker notebook instance\. A notebook instance is a machine learning \(ML\) compute instance running on a Jupyter notebook\. 

In a `CreateNotebookInstance` request, specify the type of ML compute instance that you want to run\. Amazon SageMaker launches the instance, installs common libraries that you can use to explore datasets for model training, and attaches an ML storage volume to the notebook instance\. 

Amazon SageMaker also provides a set of example notebooks\. Each notebook demonstrates how to use Amazon SageMaker with a specific algorithm or with a machine learning framework\. 

After receiving the request, Amazon SageMaker does the following:

1. Creates a network interface in the Amazon SageMaker VPC\.

1. \(Option\) If you specified `SubnetId`, Amazon SageMaker creates a network interface in your own VPC, which is inferred from the subnet ID that you provide in the input\. When creating this network interface, Amazon SageMaker attaches the security group that you specified in the request to the network interface that it creates in your VPC\.

1. Launches an EC2 instance of the type specified in the request in the Amazon SageMaker VPC\. If you specified `SubnetId` of your VPC, Amazon SageMaker specifies both network interfaces when launching this instance\. This enables inbound traffic from your own VPC to the notebook instance, assuming that the security groups allow it\.

After creating the notebook instance, Amazon SageMaker returns its Amazon Resource Name \(ARN\)\.

After Amazon SageMaker creates the notebook instance, you can connect to the Jupyter server and work in Jupyter notebooks\. For example, you can write code to explore a dataset that you can use for model training, train a model, host models by creating Amazon SageMaker endpoints, and validate hosted models\. 

For more information, see [How It Works](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works.html)\. 

## Request Syntax<a name="API_CreateNotebookInstance_RequestSyntax"></a>

```
{
   "[AcceleratorTypes](#SageMaker-CreateNotebookInstance-request-AcceleratorTypes)": [ "string" ],
   "[AdditionalCodeRepositories](#SageMaker-CreateNotebookInstance-request-AdditionalCodeRepositories)": [ "string" ],
   "[DefaultCodeRepository](#SageMaker-CreateNotebookInstance-request-DefaultCodeRepository)": "string",
   "[DirectInternetAccess](#SageMaker-CreateNotebookInstance-request-DirectInternetAccess)": "string",
   "[InstanceType](#SageMaker-CreateNotebookInstance-request-InstanceType)": "string",
   "[KmsKeyId](#SageMaker-CreateNotebookInstance-request-KmsKeyId)": "string",
   "[LifecycleConfigName](#SageMaker-CreateNotebookInstance-request-LifecycleConfigName)": "string",
   "[NotebookInstanceName](#SageMaker-CreateNotebookInstance-request-NotebookInstanceName)": "string",
   "[RoleArn](#SageMaker-CreateNotebookInstance-request-RoleArn)": "string",
   "[RootAccess](#SageMaker-CreateNotebookInstance-request-RootAccess)": "string",
   "[SecurityGroupIds](#SageMaker-CreateNotebookInstance-request-SecurityGroupIds)": [ "string" ],
   "[SubnetId](#SageMaker-CreateNotebookInstance-request-SubnetId)": "string",
   "[Tags](#SageMaker-CreateNotebookInstance-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ],
   "[VolumeSizeInGB](#SageMaker-CreateNotebookInstance-request-VolumeSizeInGB)": number
}
```

## Request Parameters<a name="API_CreateNotebookInstance_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [AcceleratorTypes](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-AcceleratorTypes"></a>
A list of Elastic Inference \(EI\) instance types to associate with this notebook instance\. Currently, only one instance type can be associated with a notebook instance\. For more information, see [Using Elastic Inference in Amazon SageMaker](http://docs.aws.amazon.com/sagemaker/latest/dg/ei.html)\.  
Type: Array of strings  
Valid Values:` ml.eia1.medium | ml.eia1.large | ml.eia1.xlarge`   
Required: No

 ** [AdditionalCodeRepositories](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-AdditionalCodeRepositories"></a>
An array of up to three Git repositories to associate with the notebook instance\. These can be either the names of Git repositories stored as resources in your account, or the URL of Git repositories in [AWS CodeCommit](http://docs.aws.amazon.com/codecommit/latest/userguide/welcome.html) or in any other Git repository\. These repositories are cloned at the same level as the default repository of your notebook instance\. For more information, see [Associating Git Repositories with Amazon SageMaker Notebook Instances](http://docs.aws.amazon.com/sagemaker/latest/dg/nbi-git-repo.html)\.  
Type: Array of strings  
Array Members: Maximum number of 3 items\.  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `^https://([^/]+)/?(.*)$|^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 ** [DefaultCodeRepository](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-DefaultCodeRepository"></a>
A Git repository to associate with the notebook instance as its default code repository\. This can be either the name of a Git repository stored as a resource in your account, or the URL of a Git repository in [AWS CodeCommit](http://docs.aws.amazon.com/codecommit/latest/userguide/welcome.html) or in any other Git repository\. When you open a notebook instance, it opens in the directory that contains this repository\. For more information, see [Associating Git Repositories with Amazon SageMaker Notebook Instances](http://docs.aws.amazon.com/sagemaker/latest/dg/nbi-git-repo.html)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Pattern: `^https://([^/]+)/?(.*)$|^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 ** [DirectInternetAccess](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-DirectInternetAccess"></a>
Sets whether Amazon SageMaker provides internet access to the notebook instance\. If you set this to `Disabled` this notebook instance will be able to access resources only in your VPC, and will not be able to connect to Amazon SageMaker training and endpoint services unless your configure a NAT Gateway in your VPC\.  
For more information, see [Notebook Instances Are Internet\-Enabled by Default](https://docs.aws.amazon.com/sagemaker/latest/dg/appendix-additional-considerations.html#appendix-notebook-and-internet-access)\. You can set the value of this parameter to `Disabled` only if you set a value for the `SubnetId` parameter\.  
Type: String  
Valid Values:` Enabled | Disabled`   
Required: No

 ** [InstanceType](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-InstanceType"></a>
The type of ML compute instance to launch for the notebook instance\.  
Type: String  
Valid Values:` ml.t2.medium | ml.t2.large | ml.t2.xlarge | ml.t2.2xlarge | ml.t3.medium | ml.t3.large | ml.t3.xlarge | ml.t3.2xlarge | ml.m4.xlarge | ml.m4.2xlarge | ml.m4.4xlarge | ml.m4.10xlarge | ml.m4.16xlarge | ml.m5.xlarge | ml.m5.2xlarge | ml.m5.4xlarge | ml.m5.12xlarge | ml.m5.24xlarge | ml.c4.xlarge | ml.c4.2xlarge | ml.c4.4xlarge | ml.c4.8xlarge | ml.c5.xlarge | ml.c5.2xlarge | ml.c5.4xlarge | ml.c5.9xlarge | ml.c5.18xlarge | ml.c5d.xlarge | ml.c5d.2xlarge | ml.c5d.4xlarge | ml.c5d.9xlarge | ml.c5d.18xlarge | ml.p2.xlarge | ml.p2.8xlarge | ml.p2.16xlarge | ml.p3.2xlarge | ml.p3.8xlarge | ml.p3.16xlarge`   
Required: Yes

 ** [KmsKeyId](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-KmsKeyId"></a>
 If you provide a AWS KMS key ID, Amazon SageMaker uses it to encrypt data at rest on the ML storage volume that is attached to your notebook instance\. The KMS key you provide must be enabled\. For information, see [Enabling and Disabling Keys](http://docs.aws.amazon.com/kms/latest/developerguide/enabling-keys.html) in the *AWS Key Management Service Developer Guide*\.  
Type: String  
Length Constraints: Maximum length of 2048\.  
Pattern: `.*`   
Required: No

 ** [LifecycleConfigName](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-LifecycleConfigName"></a>
The name of a lifecycle configuration to associate with the notebook instance\. For information about lifestyle configurations, see [Step 2\.1: \(Optional\) Customize a Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/notebook-lifecycle-config.html)\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 ** [NotebookInstanceName](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-NotebookInstanceName"></a>
The name of the new notebook instance\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [RoleArn](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-RoleArn"></a>
 When you send any requests to AWS resources from the notebook instance, Amazon SageMaker assumes this role to perform tasks on your behalf\. You must grant this role necessary permissions so Amazon SageMaker can perform these tasks\. The policy must allow the Amazon SageMaker service principal \(sagemaker\.amazonaws\.com\) permissions to assume this role\. For more information, see [Amazon SageMaker Roles](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)\.   
To be able to pass this role to Amazon SageMaker, the caller of this API must have the `iam:PassRole` permission\.
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: Yes

 ** [RootAccess](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-RootAccess"></a>
Whether root access is enabled or disabled for users of the notebook instance\. The default value is `Enabled`\.  
Lifecycle configurations need root access to be able to set up a notebook instance\. Because of this, lifecycle configurations associated with a notebook instance always run with root access even if you disable root access for users\.
Type: String  
Valid Values:` Enabled | Disabled`   
Required: No

 ** [SecurityGroupIds](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-SecurityGroupIds"></a>
The VPC security group IDs, in the form sg\-xxxxxxxx\. The security groups must be for the same VPC as specified in the subnet\.   
Type: Array of strings  
Array Members: Maximum number of 5 items\.  
Length Constraints: Maximum length of 32\.  
Pattern: `[-0-9a-zA-Z]+`   
Required: No

 ** [SubnetId](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-SubnetId"></a>
The ID of the subnet in a VPC to which you would like to have a connectivity from your ML compute instance\.   
Type: String  
Length Constraints: Maximum length of 32\.  
Pattern: `[-0-9a-zA-Z]+`   
Required: No

 ** [Tags](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-Tags"></a>
A list of tags to associate with the notebook instance\. You can add tags later by using the `CreateTags` API\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

 ** [VolumeSizeInGB](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-VolumeSizeInGB"></a>
The size, in GB, of the ML storage volume to attach to the notebook instance\. The default value is 5 GB\.  
Type: Integer  
Valid Range: Minimum value of 5\. Maximum value of 16384\.  
Required: No

## Response Syntax<a name="API_CreateNotebookInstance_ResponseSyntax"></a>

```
{
   "[NotebookInstanceArn](#SageMaker-CreateNotebookInstance-response-NotebookInstanceArn)": "string"
}
```

## Response Elements<a name="API_CreateNotebookInstance_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [NotebookInstanceArn](#API_CreateNotebookInstance_ResponseSyntax) **   <a name="SageMaker-CreateNotebookInstance-response-NotebookInstanceArn"></a>
The Amazon Resource Name \(ARN\) of the notebook instance\.   
Type: String  
Length Constraints: Maximum length of 256\.

## Errors<a name="API_CreateNotebookInstance_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_CreateNotebookInstance_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateNotebookInstance) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateNotebookInstance) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateNotebookInstance) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateNotebookInstance) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/CreateNotebookInstance) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateNotebookInstance) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateNotebookInstance) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateNotebookInstance) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateNotebookInstance) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateNotebookInstance) 