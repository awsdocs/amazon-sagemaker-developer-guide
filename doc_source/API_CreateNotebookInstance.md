# CreateNotebookInstance<a name="API_CreateNotebookInstance"></a>

Creates an Amazon SageMaker notebook instance\. A notebook instance is an ML compute instance running on a Jupyter notebook\. 

In a `CreateNotebookInstance` request, you specify the type of ML compute instance that you want to run\. Amazon SageMaker launches the instance, installs common libraries that you can use to explore datasets for model training, and attaches an ML storage volume to the notebook instance\. 

Amazon SageMaker also provides a set of example notebooks\. Each notebook demonstrates how to use Amazon SageMaker with a specific an algorithm or with a machine learning framework\. 

After receiving the request, Amazon SageMaker does the following:

1. Creates a network interface in the Amazon SageMaker VPC\.

1. \(Option\) If you specified `SubnetId`, creates a network interface in your own VPC, which is inferred from the subnet ID that you provide in the input\. When creating this network interface, Amazon SageMaker attaches the security group that you specified in the request to the network interface that it creates in your VPC\.

1. Launches an EC2 instance of the type specified in the request in the Amazon SageMaker VPC\. If you specified `SubnetId` of your VPC, Amazon SageMaker specifies both network interfaces when launching this instance\. This enables inbound traffic from your own VPC to the notebook instance, assuming that the security groups allow it\.

After creating the notebook instance, Amazon SageMaker returns its Amazon Resource Name \(ARN\)\.

After Amazon SageMaker creates the notebook instance, you can connect to the Jupyter server and work in Jupyter notebooks\. For example, you can write code to explore a dataset that you can use for model training, train a model, host models by creating Amazon SageMaker endpoints, and validate hosted models\. 

For more information, see [How It Works](http://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works.html)\. 

## Request Syntax<a name="API_CreateNotebookInstance_RequestSyntax"></a>

```
{
   "[InstanceType](#SageMaker-CreateNotebookInstance-request-InstanceType)": "string",
   "[KmsKeyId](#SageMaker-CreateNotebookInstance-request-KmsKeyId)": "string",
   "[NotebookInstanceName](#SageMaker-CreateNotebookInstance-request-NotebookInstanceName)": "string",
   "[RoleArn](#SageMaker-CreateNotebookInstance-request-RoleArn)": "string",
   "[SecurityGroupIds](#SageMaker-CreateNotebookInstance-request-SecurityGroupIds)": [ "string" ],
   "[SubnetId](#SageMaker-CreateNotebookInstance-request-SubnetId)": "string",
   "[Tags](#SageMaker-CreateNotebookInstance-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ]
}
```

## Request Parameters<a name="API_CreateNotebookInstance_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [InstanceType](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-InstanceType"></a>
The type of ML compute instance to launch for the notebook instance\.  
Type: String  
Valid Values:` ml.t2.medium | ml.m4.xlarge | ml.p2.xlarge`   
Required: Yes

 ** [KmsKeyId](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-KmsKeyId"></a>
 If you provide a AWS KMS key ID, Amazon SageMaker uses it to encrypt data at rest on the ML storage volume that is attached to your notebook instance\.   
Type: String  
Length Constraints: Maximum length of 2048\.  
Required: No

 ** [NotebookInstanceName](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-NotebookInstanceName"></a>
The name of the new notebook instance\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [RoleArn](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-RoleArn"></a>
 When you send any requests to AWS resources from the notebook instance, Amazon SageMaker assumes this role to perform tasks on your behalf\. You must grant this role necessary permissions so Amazon SageMaker can perform these tasks\. The policy must allow the Amazon SageMaker service principal \(sagemaker\.amazonaws\.com\) permissions to assume this role\. For more information, see [Amazon SageMaker Roles](http://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html)\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$`   
Required: Yes

 ** [SecurityGroupIds](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-SecurityGroupIds"></a>
The VPC security group IDs, in the form sg\-xxxxxxxx\. The security groups must be for the same VPC as specified in the subnet\.   
Type: Array of strings  
Array Members: Maximum number of 5 items\.  
Length Constraints: Maximum length of 32\.  
Required: No

 ** [SubnetId](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-SubnetId"></a>
The ID of the subnet in a VPC to which you would like to have a connectivity from your ML compute instance\.   
Type: String  
Length Constraints: Maximum length of 32\.  
Required: No

 ** [Tags](#API_CreateNotebookInstance_RequestSyntax) **   <a name="SageMaker-CreateNotebookInstance-request-Tags"></a>
A list of tags to associate with the notebook instance\. You can add tags later by using the `CreateTags` API\.  
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
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

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateNotebookInstance) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateNotebookInstance) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateNotebookInstance) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateNotebookInstance) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateNotebookInstance) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateNotebookInstance) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateNotebookInstance) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateNotebookInstance) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateNotebookInstance) 