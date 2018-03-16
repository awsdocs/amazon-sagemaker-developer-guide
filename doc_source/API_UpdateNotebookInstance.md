# UpdateNotebookInstance<a name="API_UpdateNotebookInstance"></a>

Updates a notebook instance\. NotebookInstance updates include upgrading or downgrading the ML compute instance used for your notebook instance to accommodate changes in your workload requirements\. You can also update the VPC security groups\.

## Request Syntax<a name="API_UpdateNotebookInstance_RequestSyntax"></a>

```
{
   "[InstanceType](#SageMaker-UpdateNotebookInstance-request-InstanceType)": "string",
   "[NotebookInstanceName](#SageMaker-UpdateNotebookInstance-request-NotebookInstanceName)": "string",
   "[RoleArn](#SageMaker-UpdateNotebookInstance-request-RoleArn)": "string"
}
```

## Request Parameters<a name="API_UpdateNotebookInstance_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [InstanceType](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-InstanceType"></a>
The Amazon ML compute instance type\.  
Type: String  
Valid Values:` ml.t2.medium | ml.m4.xlarge | ml.p2.xlarge | ml.p3.2xlarge`   
Required: No

 ** [NotebookInstanceName](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-NotebookInstanceName"></a>
The name of the notebook instance to update\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** [RoleArn](#API_UpdateNotebookInstance_RequestSyntax) **   <a name="SageMaker-UpdateNotebookInstance-request-RoleArn"></a>
Amazon Resource Name \(ARN\) of the IAM role to associate with the instance\.  
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

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/UpdateNotebookInstance) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/UpdateNotebookInstance) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/UpdateNotebookInstance) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/UpdateNotebookInstance) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/UpdateNotebookInstance) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/UpdateNotebookInstance) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/UpdateNotebookInstance) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/UpdateNotebookInstance) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/UpdateNotebookInstance) 