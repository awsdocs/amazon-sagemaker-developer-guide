# CreateWorkteam<a name="API_CreateWorkteam"></a>

Creates a new work team for labeling your data\. A work team is defined by one or more Amazon Cognito user pools\. You must first create the user pools before you can create a work team\.

You cannot create more than 25 work teams in an account and region\.

## Request Syntax<a name="API_CreateWorkteam_RequestSyntax"></a>

```
{
   "[Description](#SageMaker-CreateWorkteam-request-Description)": "string",
   "[MemberDefinitions](#SageMaker-CreateWorkteam-request-MemberDefinitions)": [ 
      { 
         "[CognitoMemberDefinition](API_MemberDefinition.md#SageMaker-Type-MemberDefinition-CognitoMemberDefinition)": { 
            "[ClientId](API_CognitoMemberDefinition.md#SageMaker-Type-CognitoMemberDefinition-ClientId)": "string",
            "[UserGroup](API_CognitoMemberDefinition.md#SageMaker-Type-CognitoMemberDefinition-UserGroup)": "string",
            "[UserPool](API_CognitoMemberDefinition.md#SageMaker-Type-CognitoMemberDefinition-UserPool)": "string"
         }
      }
   ],
   "[Tags](#SageMaker-CreateWorkteam-request-Tags)": [ 
      { 
         "[Key](API_Tag.md#SageMaker-Type-Tag-Key)": "string",
         "[Value](API_Tag.md#SageMaker-Type-Tag-Value)": "string"
      }
   ],
   "[WorkteamName](#SageMaker-CreateWorkteam-request-WorkteamName)": "string"
}
```

## Request Parameters<a name="API_CreateWorkteam_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [Description](#API_CreateWorkteam_RequestSyntax) **   <a name="SageMaker-CreateWorkteam-request-Description"></a>
A description of the work team\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 200\.  
Required: Yes

 ** [MemberDefinitions](#API_CreateWorkteam_RequestSyntax) **   <a name="SageMaker-CreateWorkteam-request-MemberDefinitions"></a>
A list of `MemberDefinition` objects that contains objects that identify the Amazon Cognito user pool that makes up the work team\. For more information, see [Amazon Cognito User Pools](http://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html)\.  
All of the `CognitoMemberDefinition` objects that make up the member definition must have the same `ClientId` and `UserPool` values\.  
Type: Array of [MemberDefinition](API_MemberDefinition.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 10 items\.  
Required: Yes

 ** [Tags](#API_CreateWorkteam_RequestSyntax) **   <a name="SageMaker-CreateWorkteam-request-Tags"></a>
Type: Array of [Tag](API_Tag.md) objects  
Array Members: Minimum number of 0 items\. Maximum number of 50 items\.  
Required: No

 ** [WorkteamName](#API_CreateWorkteam_RequestSyntax) **   <a name="SageMaker-CreateWorkteam-request-WorkteamName"></a>
The name of the work team\. Use this name to identify the work team\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_CreateWorkteam_ResponseSyntax"></a>

```
{
   "[WorkteamArn](#SageMaker-CreateWorkteam-response-WorkteamArn)": "string"
}
```

## Response Elements<a name="API_CreateWorkteam_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [WorkteamArn](#API_CreateWorkteam_ResponseSyntax) **   <a name="SageMaker-CreateWorkteam-response-WorkteamArn"></a>
The Amazon Resource Name \(ARN\) of the work team\. You can use this ARN to identify the work team\.  
Type: String  
Length Constraints: Maximum length of 256\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:workteam/.*` 

## Errors<a name="API_CreateWorkteam_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceInUse**   
Resource being accessed is in use\.  
HTTP Status Code: 400

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_CreateWorkteam_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateWorkteam) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateWorkteam) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateWorkteam) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateWorkteam) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateWorkteam) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateWorkteam) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateWorkteam) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateWorkteam) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateWorkteam) 