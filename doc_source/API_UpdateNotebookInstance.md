# UpdateNotebookInstance<a name="API_UpdateNotebookInstance"></a>

Updates a notebook instance\. NotebookInstance updates include upgrading or downgrading the ML compute instance used for your notebook instance to accommodate changes in your workload requirements\. You can also update the VPC security groups\.

## Request Syntax<a name="API_UpdateNotebookInstance_RequestSyntax"></a>

```
{
   "[DisassociateLifecycleConfig](#SageMaker-UpdateNotebookInstance-request-DisassociateLifecycleConfig)": boolean,
   "[InstanceType](#SageMaker-UpdateNotebookInstance-request-InstanceType)": "string",
   "[LifecycleConfigName](#SageMaker-UpdateNotebookInstance-request-LifecycleConfigName)": "string",
   "[NotebookInstanceName](#SageMaker-UpdateNotebookInstance-request-NotebookInstanceName)": "string",
   "[RoleArn](#SageMaker-UpdateNotebookInstance-request-RoleArn)": "string"
}
```

## Request Parameters<a name="API_UpdateNotebookInstance_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [DisassociateLifecycleConfig](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-DisassociateLifecycleConfig"></a>
Type: Boolean  
Required: No

 ** [InstanceType](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-InstanceType"></a>
The Amazon ML compute instance type\.  
Type: String  
Valid Values:` ml.t2.medium | ml.t2.large | ml.t2.xlarge | ml.t2.2xlarge | ml.m4.xlarge | ml.m4.2xlarge | ml.m4.4xlarge | ml.m4.10xlarge | ml.m4.16xlarge | ml.p2.xlarge | ml.p2.8xlarge | ml.p2.16xlarge | ml.p3.2xlarge | ml.p3.8xlarge | ml.p3.16xlarge`   
Required: No

 ** [LifecycleConfigName](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-LifecycleConfigName"></a>
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
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/UpdateNotebookInstance) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/UpdateNotebookInstance) 