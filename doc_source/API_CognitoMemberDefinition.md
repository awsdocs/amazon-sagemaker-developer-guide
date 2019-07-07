# CognitoMemberDefinition<a name="API_CognitoMemberDefinition"></a>

Identifies a Amazon Cognito user group\. A user group can be used in on or more work teams\.

## Contents<a name="API_CognitoMemberDefinition_Contents"></a>

 **ClientId**   <a name="SageMaker-Type-CognitoMemberDefinition-ClientId"></a>
An identifier for an application client\. You must create the app client ID using Amazon Cognito\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `[\w+]+`   
Required: Yes

 **UserGroup**   <a name="SageMaker-Type-CognitoMemberDefinition-UserGroup"></a>
An identifier for a user group\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `[\p{L}\p{M}\p{S}\p{N}\p{P}]+`   
Required: Yes

 **UserPool**   <a name="SageMaker-Type-CognitoMemberDefinition-UserPool"></a>
An identifier for a user pool\. The user pool must be in the same region as the service that you are calling\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 55\.  
Pattern: `[\w-]+_[0-9a-zA-Z]+`   
Required: Yes

## See Also<a name="API_CognitoMemberDefinition_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CognitoMemberDefinition) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CognitoMemberDefinition) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/CognitoMemberDefinition) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CognitoMemberDefinition) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CognitoMemberDefinition) 