# UpdateWorkteam<a name="API_UpdateWorkteam"></a>

Updates an existing work team with new member definitions or description\.

## Request Syntax<a name="API_UpdateWorkteam_RequestSyntax"></a>

```
{
   "[Description](#SageMaker-UpdateWorkteam-request-Description)": "string",
   "[MemberDefinitions](#SageMaker-UpdateWorkteam-request-MemberDefinitions)": [ 
      { 
         "[CognitoMemberDefinition](API_MemberDefinition.md#SageMaker-Type-MemberDefinition-CognitoMemberDefinition)": { 
            "[ClientId](API_CognitoMemberDefinition.md#SageMaker-Type-CognitoMemberDefinition-ClientId)": "string",
            "[UserGroup](API_CognitoMemberDefinition.md#SageMaker-Type-CognitoMemberDefinition-UserGroup)": "string",
            "[UserPool](API_CognitoMemberDefinition.md#SageMaker-Type-CognitoMemberDefinition-UserPool)": "string"
         }
      }
   ],
   "[WorkteamName](#SageMaker-UpdateWorkteam-request-WorkteamName)": "string"
}
```

## Request Parameters<a name="API_UpdateWorkteam_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [Description](#API_UpdateWorkteam_RequestSyntax) **   <a name="SageMaker-UpdateWorkteam-request-Description"></a>
An updated description for the work team\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 200\.  
Required: No

 ** [MemberDefinitions](#API_UpdateWorkteam_RequestSyntax) **   <a name="SageMaker-UpdateWorkteam-request-MemberDefinitions"></a>
A list of `MemberDefinition` objects that contain the updated work team members\.  
Type: Array of [MemberDefinition](API_MemberDefinition.md) objects  
Array Members: Minimum number of 1 item\. Maximum number of 10 items\.  
Required: No

 ** [WorkteamName](#API_UpdateWorkteam_RequestSyntax) **   <a name="SageMaker-UpdateWorkteam-request-WorkteamName"></a>
The name of the work team to update\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_UpdateWorkteam_ResponseSyntax"></a>

```
{
   "[Workteam](#SageMaker-UpdateWorkteam-response-Workteam)": { 
      "[CreateDate](API_Workteam.md#SageMaker-Type-Workteam-CreateDate)": number,
      "[Description](API_Workteam.md#SageMaker-Type-Workteam-Description)": "string",
      "[LastUpdatedDate](API_Workteam.md#SageMaker-Type-Workteam-LastUpdatedDate)": number,
      "[MemberDefinitions](API_Workteam.md#SageMaker-Type-Workteam-MemberDefinitions)": [ 
         { 
            "[CognitoMemberDefinition](API_MemberDefinition.md#SageMaker-Type-MemberDefinition-CognitoMemberDefinition)": { 
               "[ClientId](API_CognitoMemberDefinition.md#SageMaker-Type-CognitoMemberDefinition-ClientId)": "string",
               "[UserGroup](API_CognitoMemberDefinition.md#SageMaker-Type-CognitoMemberDefinition-UserGroup)": "string",
               "[UserPool](API_CognitoMemberDefinition.md#SageMaker-Type-CognitoMemberDefinition-UserPool)": "string"
            }
         }
      ],
      "[ProductListingIds](API_Workteam.md#SageMaker-Type-Workteam-ProductListingIds)": [ "string" ],
      "[SubDomain](API_Workteam.md#SageMaker-Type-Workteam-SubDomain)": "string",
      "[WorkteamArn](API_Workteam.md#SageMaker-Type-Workteam-WorkteamArn)": "string",
      "[WorkteamName](API_Workteam.md#SageMaker-Type-Workteam-WorkteamName)": "string"
   }
}
```

## Response Elements<a name="API_UpdateWorkteam_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [Workteam](#API_UpdateWorkteam_ResponseSyntax) **   <a name="SageMaker-UpdateWorkteam-response-Workteam"></a>
A `Workteam` object that describes the updated work team\.  
Type: [Workteam](API_Workteam.md) object

## Errors<a name="API_UpdateWorkteam_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ResourceLimitExceeded**   
 You have exceeded an Amazon SageMaker resource limit\. For example, you might have too many training jobs created\.   
HTTP Status Code: 400

## See Also<a name="API_UpdateWorkteam_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/UpdateWorkteam) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/UpdateWorkteam) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/UpdateWorkteam) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/UpdateWorkteam) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/UpdateWorkteam) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/UpdateWorkteam) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/UpdateWorkteam) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/UpdateWorkteam) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/UpdateWorkteam) 