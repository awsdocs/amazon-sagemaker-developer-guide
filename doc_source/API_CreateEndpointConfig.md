# CreateEndpointConfig<a name="API_CreateEndpointConfig"></a>

Creates an endpoint configuration that Amazon SageMaker hosting services uses to deploy models\. In the configuration, you identify one or more models, created using the `CreateModel` API, to deploy and the resources that you want Amazon SageMaker to provision\. Then you call the [CreateEndpoint](http://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html) API\. 

**Note**  
 Use this API only if you want to use Amazon SageMaker hosting services to deploy models into production\. 

In the request, you define one or more `ProductionVariant`s, each of which identifies a model\. Each `ProductionVariant` parameter also describes the resources that you want Amazon SageMaker to provision\. This includes the number and type of ML compute instances to deploy\. 

If you are hosting multiple models, you also assign a `VariantWeight` to specify how much traffic you want to allocate to each model\. For example, suppose that you want to host two models, A and B, and you assign traffic weight 2 for model A and 1 for model B\. Amazon SageMaker distributes two\-thirds of the traffic to Model A, and one\-third to model B\. 

## Request Syntax<a name="API_CreateEndpointConfig_RequestSyntax"></a>

```
{
   "EndpointConfigName": "string",
   "KmsKeyId": "string",
   "ProductionVariants": [ 
      { 
         "InitialInstanceCount": number,
         "InitialVariantWeight": number,
         "InstanceType": "string",
         "ModelName": "string",
         "VariantName": "string"
      }
   ],
   "Tags": [ 
      { 
         "Key": "string",
         "Value": "string"
      }
   ]
}
```

## Request Parameters<a name="API_CreateEndpointConfig_RequestParameters"></a>

For information about the parameters that are common to all actions, see Common Parameters\.

The request accepts the following data in JSON format\.

 ** EndpointConfigName **   
The name of the endpoint configuration\. You specify this name in a [CreateEndpoint](http://docs.aws.amazon.com/sagemaker/latest/dg/API_CreateEndpoint.html) request\.   
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

 ** KmsKeyId **   
The Amazon Resource Name \(ARN\) of a AWS Key Management Service key that Amazon SageMaker uses to encrypt data on the storage volume attached to the ML compute instance that hosts the endpoint\.  
Type: String  
Length Constraints: Maximum length of 2048\.  
Required: No

 ** ProductionVariants **   
An array of `ProductionVariant` objects, one for each model that you want to host at this endpoint\.  
Type: Array of [ProductionVariant](API_ProductionVariant.md) objects  
Array Members: Minimum number of 1 item\.  
Required: Yes

 ** Tags **   
An array of key\-value pairs\. For more information, see [Using Cost Allocation Tags](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html#allocation-what) in the *AWS Billing and Cost Management User Guide*\.   
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

## Response Syntax<a name="API_CreateEndpointConfig_ResponseSyntax"></a>

```
{
   "EndpointConfigArn": "string"
}
```

## Response Elements<a name="API_CreateEndpointConfig_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** EndpointConfigArn **   
The Amazon Resource Name \(ARN\) of the endpoint configuration\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.

## Errors<a name="API_CreateEndpointConfig_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_CreateEndpointConfig_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateEndpointConfig) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateEndpointConfig) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateEndpointConfig) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateEndpointConfig) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateEndpointConfig) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateEndpointConfig) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateEndpointConfig) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateEndpointConfig) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateEndpointConfig) 