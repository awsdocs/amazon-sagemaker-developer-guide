# DescribeEndpointConfig<a name="API_DescribeEndpointConfig"></a>

Returns the description of an endpoint configuration created using the `CreateEndpointConfig` API\.

## Request Syntax<a name="API_DescribeEndpointConfig_RequestSyntax"></a>

```
{
   "[EndpointConfigName](#SageMaker-DescribeEndpointConfig-request-EndpointConfigName)": "string"
}
```

## Request Parameters<a name="API_DescribeEndpointConfig_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [EndpointConfigName](#API_DescribeEndpointConfig_RequestSyntax) **   <a name="SageMaker-DescribeEndpointConfig-request-EndpointConfigName"></a>
The name of the endpoint configuration\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeEndpointConfig_ResponseSyntax"></a>

```
{
   "[CreationTime](#SageMaker-DescribeEndpointConfig-response-CreationTime)": number,
   "[EndpointConfigArn](#SageMaker-DescribeEndpointConfig-response-EndpointConfigArn)": "string",
   "[EndpointConfigName](#SageMaker-DescribeEndpointConfig-response-EndpointConfigName)": "string",
   "[KmsKeyId](#SageMaker-DescribeEndpointConfig-response-KmsKeyId)": "string",
   "[ProductionVariants](#SageMaker-DescribeEndpointConfig-response-ProductionVariants)": [ 
      { 
         "[InitialInstanceCount](API_ProductionVariant.md#SageMaker-Type-ProductionVariant-InitialInstanceCount)": number,
         "[InitialVariantWeight](API_ProductionVariant.md#SageMaker-Type-ProductionVariant-InitialVariantWeight)": number,
         "[InstanceType](API_ProductionVariant.md#SageMaker-Type-ProductionVariant-InstanceType)": "string",
         "[ModelName](API_ProductionVariant.md#SageMaker-Type-ProductionVariant-ModelName)": "string",
         "[VariantName](API_ProductionVariant.md#SageMaker-Type-ProductionVariant-VariantName)": "string"
      }
   ]
}
```

## Response Elements<a name="API_DescribeEndpointConfig_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [CreationTime](#API_DescribeEndpointConfig_ResponseSyntax) **   <a name="SageMaker-DescribeEndpointConfig-response-CreationTime"></a>
A timestamp that shows when the endpoint configuration was created\.  
Type: Timestamp

 ** [EndpointConfigArn](#API_DescribeEndpointConfig_ResponseSyntax) **   <a name="SageMaker-DescribeEndpointConfig-response-EndpointConfigArn"></a>
The Amazon Resource Name \(ARN\) of the endpoint configuration\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.

 ** [EndpointConfigName](#API_DescribeEndpointConfig_ResponseSyntax) **   <a name="SageMaker-DescribeEndpointConfig-response-EndpointConfigName"></a>
Name of the Amazon SageMaker endpoint configuration\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*` 

 ** [KmsKeyId](#API_DescribeEndpointConfig_ResponseSyntax) **   <a name="SageMaker-DescribeEndpointConfig-response-KmsKeyId"></a>
AWS KMS key ID Amazon SageMaker uses to encrypt data when storing it on the ML storage volume attached to the instance\.  
Type: String  
Length Constraints: Maximum length of 2048\.

 ** [ProductionVariants](#API_DescribeEndpointConfig_ResponseSyntax) **   <a name="SageMaker-DescribeEndpointConfig-response-ProductionVariants"></a>
An array of `ProductionVariant` objects, one for each model that you want to host at this endpoint\.  
Type: Array of [ProductionVariant](API_ProductionVariant.md) objects  
Array Members: Minimum number of 1 item\.

## Errors<a name="API_DescribeEndpointConfig_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DescribeEndpointConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeEndpointConfig) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeEndpointConfig) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeEndpointConfig) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeEndpointConfig) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeEndpointConfig) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeEndpointConfig) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeEndpointConfig) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeEndpointConfig) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeEndpointConfig) 