# DescribeSubscribedWorkteam<a name="API_DescribeSubscribedWorkteam"></a>

Gets information about a work team provided by a vendor\. It returns details about the subscription with a vendor in the AWS Marketplace\.

## Request Syntax<a name="API_DescribeSubscribedWorkteam_RequestSyntax"></a>

```
{
   "[WorkteamArn](#SageMaker-DescribeSubscribedWorkteam-request-WorkteamArn)": "string"
}
```

## Request Parameters<a name="API_DescribeSubscribedWorkteam_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [WorkteamArn](#API_DescribeSubscribedWorkteam_RequestSyntax) **   <a name="SageMaker-DescribeSubscribedWorkteam-request-WorkteamArn"></a>
The Amazon Resource Name \(ARN\) of the subscribed work team to describe\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:workteam/.*`   
Required: Yes

## Response Syntax<a name="API_DescribeSubscribedWorkteam_ResponseSyntax"></a>

```
{
   "[SubscribedWorkteam](#SageMaker-DescribeSubscribedWorkteam-response-SubscribedWorkteam)": { 
      "[ListingId](API_SubscribedWorkteam.md#SageMaker-Type-SubscribedWorkteam-ListingId)": "string",
      "[MarketplaceDescription](API_SubscribedWorkteam.md#SageMaker-Type-SubscribedWorkteam-MarketplaceDescription)": "string",
      "[MarketplaceTitle](API_SubscribedWorkteam.md#SageMaker-Type-SubscribedWorkteam-MarketplaceTitle)": "string",
      "[SellerName](API_SubscribedWorkteam.md#SageMaker-Type-SubscribedWorkteam-SellerName)": "string",
      "[WorkteamArn](API_SubscribedWorkteam.md#SageMaker-Type-SubscribedWorkteam-WorkteamArn)": "string"
   }
}
```

## Response Elements<a name="API_DescribeSubscribedWorkteam_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [SubscribedWorkteam](#API_DescribeSubscribedWorkteam_ResponseSyntax) **   <a name="SageMaker-DescribeSubscribedWorkteam-response-SubscribedWorkteam"></a>
A `Workteam` instance that contains information about the work team\.  
Type: [SubscribedWorkteam](API_SubscribedWorkteam.md) object

## Errors<a name="API_DescribeSubscribedWorkteam_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DescribeSubscribedWorkteam_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeSubscribedWorkteam) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeSubscribedWorkteam) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeSubscribedWorkteam) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeSubscribedWorkteam) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DescribeSubscribedWorkteam) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeSubscribedWorkteam) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeSubscribedWorkteam) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeSubscribedWorkteam) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeSubscribedWorkteam) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeSubscribedWorkteam) 