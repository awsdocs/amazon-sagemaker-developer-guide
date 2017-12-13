# UpdateEndpointWeightsAndCapacities<a name="API_UpdateEndpointWeightsAndCapacities"></a>

Updates variant weight, capacity, or both of one or more variants associated with an endpoint\. This operation updates weight, capacity, or both for the previously provisioned endpoint\. When it receives the request, Amazon SageMaker sets the endpoint status to `Updating`\. After updating the endpoint, it sets the status to `InService`\. To check the status of an endpoint, use the [DescribeEndpoint](http://docs.aws.amazon.com/sagemaker/latest/dg/API_DescribeEndpoint.html) API\. 

## Request Syntax<a name="API_UpdateEndpointWeightsAndCapacities_RequestSyntax"></a>

```
{
   "DesiredWeightsAndCapacities": [ 
      { 
         "DesiredInstanceCount": number,
         "DesiredWeight": number,
         "VariantName": "string"
      }
   ],
   "EndpointName": "string"
}
```

## Request Parameters<a name="API_UpdateEndpointWeightsAndCapacities_RequestParameters"></a>

For information about the parameters that are common to all actions, see Common Parameters\.

The request accepts the following data in JSON format\.

 ** DesiredWeightsAndCapacities **   
An object that provides new capacity and weight values for a variant\.  
Type: Array of [DesiredWeightAndCapacity](API_DesiredWeightAndCapacity.md) objects  
Array Members: Minimum number of 1 item\.  
Required: Yes

 ** EndpointName **   
The name of an existing Amazon SageMaker endpoint\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_UpdateEndpointWeightsAndCapacities_ResponseSyntax"></a>

```
{
   "EndpointArn": "string"
}
```

## Response Elements<a name="API_UpdateEndpointWeightsAndCapacities_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** EndpointArn **   
The Amazon Resource Name \(ARN\) of the updated endpoint\.  
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.

## Errors<a name="API_UpdateEndpointWeightsAndCapacities_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_UpdateEndpointWeightsAndCapacities_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/UpdateEndpointWeightsAndCapacities) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/UpdateEndpointWeightsAndCapacities) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/UpdateEndpointWeightsAndCapacities) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/UpdateEndpointWeightsAndCapacities) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/UpdateEndpointWeightsAndCapacities) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/UpdateEndpointWeightsAndCapacities) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/UpdateEndpointWeightsAndCapacities) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/UpdateEndpointWeightsAndCapacities) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/UpdateEndpointWeightsAndCapacities) 