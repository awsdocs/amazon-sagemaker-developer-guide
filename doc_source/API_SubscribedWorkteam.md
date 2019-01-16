# SubscribedWorkteam<a name="API_SubscribedWorkteam"></a>

Describes a work team of a vendor that does the a labelling job\.

## Contents<a name="API_SubscribedWorkteam_Contents"></a>

 **ListingId**   <a name="SageMaker-Type-SubscribedWorkteam-ListingId"></a>
Type: String  
Required: No

 **MarketplaceDescription**   <a name="SageMaker-Type-SubscribedWorkteam-MarketplaceDescription"></a>
The description of the vendor from the Amazon Marketplace\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 200\.  
Required: No

 **MarketplaceTitle**   <a name="SageMaker-Type-SubscribedWorkteam-MarketplaceTitle"></a>
The title of the service provided by the vendor in the Amazon Marketplace\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 200\.  
Required: No

 **SellerName**   <a name="SageMaker-Type-SubscribedWorkteam-SellerName"></a>
The name of the vendor in the Amazon Marketplace\.  
Type: String  
Required: No

 **WorkteamArn**   <a name="SageMaker-Type-SubscribedWorkteam-WorkteamArn"></a>
The Amazon Resource Name \(ARN\) of the vendor that you have subscribed\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:workteam/.*`   
Required: Yes

## See Also<a name="API_SubscribedWorkteam_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/SubscribedWorkteam) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/SubscribedWorkteam) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/SubscribedWorkteam) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/SubscribedWorkteam) 