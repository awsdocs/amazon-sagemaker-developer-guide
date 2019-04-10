# Workteam<a name="API_Workteam"></a>

Provides details about a labeling work team\.

## Contents<a name="API_Workteam_Contents"></a>

 **CreateDate**   <a name="SageMaker-Type-Workteam-CreateDate"></a>
The date and time that the work team was created \(timestamp\)\.  
Type: Timestamp  
Required: No

 **Description**   <a name="SageMaker-Type-Workteam-Description"></a>
A description of the work team\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 200\.  
Pattern: `.+`   
Required: Yes

 **LastUpdatedDate**   <a name="SageMaker-Type-Workteam-LastUpdatedDate"></a>
The date and time that the work team was last updated \(timestamp\)\.  
Type: Timestamp  
Required: No

 **MemberDefinitions**   <a name="SageMaker-Type-Workteam-MemberDefinitions"></a>
The Amazon Cognito user groups that make up the work team\.  
Type: Array of [MemberDefinition](API_MemberDefinition.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 10 items\.  
Required: Yes

 **ProductListingIds**   <a name="SageMaker-Type-Workteam-ProductListingIds"></a>
The Amazon Marketplace identifier for a vendor's work team\.  
Type: Array of strings  
Required: No

 **SubDomain**   <a name="SageMaker-Type-Workteam-SubDomain"></a>
The URI of the labeling job's user interface\. Workers open this URI to start labeling your data objects\.  
Type: String  
Required: No

 **WorkteamArn**   <a name="SageMaker-Type-Workteam-WorkteamArn"></a>
The Amazon Resource Name \(ARN\) that identifies the work team\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:workteam/.*`   
Required: Yes

 **WorkteamName**   <a name="SageMaker-Type-Workteam-WorkteamName"></a>
The name of the work team\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## See Also<a name="API_Workteam_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/Workteam) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/Workteam) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/Workteam) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/Workteam) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/Workteam) 