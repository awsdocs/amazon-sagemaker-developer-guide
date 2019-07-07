# DeleteModelPackage<a name="API_DeleteModelPackage"></a>

Deletes a model package\.

A model package is used to create Amazon SageMaker models or list on AWS Marketplace\. Buyers can subscribe to model packages listed on AWS Marketplace to create models in Amazon SageMaker\.

## Request Syntax<a name="API_DeleteModelPackage_RequestSyntax"></a>

```
{
   "[ModelPackageName](#SageMaker-DeleteModelPackage-request-ModelPackageName)": "string"
}
```

## Request Parameters<a name="API_DeleteModelPackage_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [ModelPackageName](#API_DeleteModelPackage_RequestSyntax) **   <a name="SageMaker-DeleteModelPackage-request-ModelPackageName"></a>
The name of the model package\. The name must have 1 to 63 characters\. Valid characters are a\-z, A\-Z, 0\-9, and \- \(hyphen\)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

## Response Elements<a name="API_DeleteModelPackage_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response with an empty HTTP body\.

## Errors<a name="API_DeleteModelPackage_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DeleteModelPackage_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DeleteModelPackage) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DeleteModelPackage) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DeleteModelPackage) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DeleteModelPackage) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DeleteModelPackage) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DeleteModelPackage) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DeleteModelPackage) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DeleteModelPackage) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DeleteModelPackage) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DeleteModelPackage) 