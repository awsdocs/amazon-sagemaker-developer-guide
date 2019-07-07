# ListSubscribedWorkteams<a name="API_ListSubscribedWorkteams"></a>

Gets a list of the work teams that you are subscribed to in the AWS Marketplace\. The list may be empty if no work team satisfies the filter specified in the `NameContains` parameter\.

## Request Syntax<a name="API_ListSubscribedWorkteams_RequestSyntax"></a>

```
{
   "[MaxResults](#SageMaker-ListSubscribedWorkteams-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListSubscribedWorkteams-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListSubscribedWorkteams-request-NextToken)": "string"
}
```

## Request Parameters<a name="API_ListSubscribedWorkteams_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [MaxResults](#API_ListSubscribedWorkteams_RequestSyntax) **   <a name="SageMaker-ListSubscribedWorkteams-request-MaxResults"></a>
The maximum number of work teams to return in each page of the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListSubscribedWorkteams_RequestSyntax) **   <a name="SageMaker-ListSubscribedWorkteams-request-NameContains"></a>
A string in the work team name\. This filter returns only work teams whose name contains the specified string\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 ** [NextToken](#API_ListSubscribedWorkteams_RequestSyntax) **   <a name="SageMaker-ListSubscribedWorkteams-request-NextToken"></a>
If the result of the previous `ListSubscribedWorkteams` request was truncated, the response includes a `NextToken`\. To retrieve the next set of labeling jobs, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Pattern: `.*`   
Required: No

## Response Syntax<a name="API_ListSubscribedWorkteams_ResponseSyntax"></a>

```
{
   "[NextToken](#SageMaker-ListSubscribedWorkteams-response-NextToken)": "string",
   "[SubscribedWorkteams](#SageMaker-ListSubscribedWorkteams-response-SubscribedWorkteams)": [ 
      { 
         "[ListingId](API_SubscribedWorkteam.md#SageMaker-Type-SubscribedWorkteam-ListingId)": "string",
         "[MarketplaceDescription](API_SubscribedWorkteam.md#SageMaker-Type-SubscribedWorkteam-MarketplaceDescription)": "string",
         "[MarketplaceTitle](API_SubscribedWorkteam.md#SageMaker-Type-SubscribedWorkteam-MarketplaceTitle)": "string",
         "[SellerName](API_SubscribedWorkteam.md#SageMaker-Type-SubscribedWorkteam-SellerName)": "string",
         "[WorkteamArn](API_SubscribedWorkteam.md#SageMaker-Type-SubscribedWorkteam-WorkteamArn)": "string"
      }
   ]
}
```

## Response Elements<a name="API_ListSubscribedWorkteams_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [NextToken](#API_ListSubscribedWorkteams_ResponseSyntax) **   <a name="SageMaker-ListSubscribedWorkteams-response-NextToken"></a>
If the response is truncated, Amazon SageMaker returns this token\. To retrieve the next set of work teams, use it in the subsequent request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Pattern: `.*` 

 ** [SubscribedWorkteams](#API_ListSubscribedWorkteams_ResponseSyntax) **   <a name="SageMaker-ListSubscribedWorkteams-response-SubscribedWorkteams"></a>
An array of `Workteam` objects, each describing a work team\.  
Type: Array of [SubscribedWorkteam](API_SubscribedWorkteam.md) objects

## Errors<a name="API_ListSubscribedWorkteams_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListSubscribedWorkteams_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListSubscribedWorkteams) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListSubscribedWorkteams) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListSubscribedWorkteams) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListSubscribedWorkteams) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ListSubscribedWorkteams) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListSubscribedWorkteams) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListSubscribedWorkteams) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListSubscribedWorkteams) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListSubscribedWorkteams) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListSubscribedWorkteams) 