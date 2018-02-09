# DescribeModel<a name="API_DescribeModel"></a>

Describes a model that you created using the `CreateModel` API\.

## Request Syntax<a name="API_DescribeModel_RequestSyntax"></a>

```
{
   "ModelName": "string"
}
```

## Request Parameters<a name="API_DescribeModel_RequestParameters"></a>

For information about the parameters that are common to all actions, see Common Parameters\.

The request accepts the following data in JSON format\.

 ** ModelName **   
The name of the model\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeModel_ResponseSyntax"></a>

```
{
   "CreationTime": number,
   "ExecutionRoleArn": "string",
   "ModelArn": "string",
   "ModelName": "string",
   "PrimaryContainer": { 
      "ContainerHostname": "string",
      "Environment": { 
         "string" : "string" 
      },
      "Image": "string",
      "ModelDataUrl": "string"
   }
}
```

## Response Elements<a name="API_DescribeModel_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** CreationTime **   
A timestamp that shows when the model was created\.  
Type: Timestamp

 ** ExecutionRoleArn **   
The Amazon Resource Name \(ARN\) of the IAM role that you specified for the model\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Pattern: `^arn:aws[a-z\-]*:iam::\d{12}:role/?[a-zA-Z_0-9+=,.@\-_/]+$` 

 ** ModelArn **   
The Amazon Resource Name \(ARN\) of the model\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.

 ** ModelName **   
Name of the Amazon SageMaker model\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** PrimaryContainer **   
The location of the primary inference code, associated artifacts, and custom environment map that the inference code uses when it is deployed in production\.   
Type: [ContainerDefinition](API_ContainerDefinition.md) object

## Errors<a name="API_DescribeModel_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DescribeModel_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeModel) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeModel) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeModel) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeModel) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeModel) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeModel) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeModel) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeModel) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeModel) 