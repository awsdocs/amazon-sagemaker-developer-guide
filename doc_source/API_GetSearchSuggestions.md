# GetSearchSuggestions<a name="API_GetSearchSuggestions"></a>

An auto\-complete API for the search functionality in the Amazon SageMaker console\. It returns suggestions of possible matches for the property name to use in `Search` queries\. Provides suggestions for `HyperParameters`, `Tags`, and `Metrics`\.

## Request Syntax<a name="API_GetSearchSuggestions_RequestSyntax"></a>

```
{
   "[Resource](#SageMaker-GetSearchSuggestions-request-Resource)": "string",
   "[SuggestionQuery](#SageMaker-GetSearchSuggestions-request-SuggestionQuery)": { 
      "[PropertyNameQuery](API_SuggestionQuery.md#SageMaker-Type-SuggestionQuery-PropertyNameQuery)": { 
         "[PropertyNameHint](API_PropertyNameQuery.md#SageMaker-Type-PropertyNameQuery-PropertyNameHint)": "string"
      }
   }
}
```

## Request Parameters<a name="API_GetSearchSuggestions_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [Resource](#API_GetSearchSuggestions_RequestSyntax) **   <a name="SageMaker-GetSearchSuggestions-request-Resource"></a>
The name of the Amazon SageMaker resource to Search for\. The only valid `Resource` value is `TrainingJob`\.  
Type: String  
Valid Values:` TrainingJob`   
Required: Yes

 ** [SuggestionQuery](#API_GetSearchSuggestions_RequestSyntax) **   <a name="SageMaker-GetSearchSuggestions-request-SuggestionQuery"></a>
Limits the property names that are included in the response\.  
Type: [SuggestionQuery](API_SuggestionQuery.md) object  
Required: No

## Response Syntax<a name="API_GetSearchSuggestions_ResponseSyntax"></a>

```
{
   "[PropertyNameSuggestions](#SageMaker-GetSearchSuggestions-response-PropertyNameSuggestions)": [ 
      { 
         "[PropertyName](API_PropertyNameSuggestion.md#SageMaker-Type-PropertyNameSuggestion-PropertyName)": "string"
      }
   ]
}
```

## Response Elements<a name="API_GetSearchSuggestions_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [PropertyNameSuggestions](#API_GetSearchSuggestions_ResponseSyntax) **   <a name="SageMaker-GetSearchSuggestions-response-PropertyNameSuggestions"></a>
A list of property names for a `Resource` that match a `SuggestionQuery`\.  
Type: Array of [PropertyNameSuggestion](API_PropertyNameSuggestion.md) objects

## Errors<a name="API_GetSearchSuggestions_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_GetSearchSuggestions_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/GetSearchSuggestions) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/GetSearchSuggestions) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/GetSearchSuggestions) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/GetSearchSuggestions) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/GetSearchSuggestions) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/GetSearchSuggestions) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/GetSearchSuggestions) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/GetSearchSuggestions) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/GetSearchSuggestions) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/GetSearchSuggestions) 