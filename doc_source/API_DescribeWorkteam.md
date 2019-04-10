# DescribeWorkteam<a name="API_DescribeWorkteam"></a>

Gets information about a specific work team\. You can see information such as the create date, the last updated date, membership information, and the work team's Amazon Resource Name \(ARN\)\.

## Request Syntax<a name="API_DescribeWorkteam_RequestSyntax"></a>

```
{
   "[WorkteamName](#SageMaker-DescribeWorkteam-request-WorkteamName)": "string"
}
```

## Request Parameters<a name="API_DescribeWorkteam_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [WorkteamName](#API_DescribeWorkteam_RequestSyntax) **   <a name="SageMaker-DescribeWorkteam-request-WorkteamName"></a>
The name of the work team to return a description of\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: Yes

## Response Syntax<a name="API_DescribeWorkteam_ResponseSyntax"></a>

```
{
   "[Workteam](#SageMaker-DescribeWorkteam-response-Workteam)": { 
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

## Response Elements<a name="API_DescribeWorkteam_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [Workteam](#API_DescribeWorkteam_ResponseSyntax) **   <a name="SageMaker-DescribeWorkteam-response-Workteam"></a>
A `Workteam` instance that contains information about the work team\.   
Type: [Workteam](API_Workteam.md) object

## Errors<a name="API_DescribeWorkteam_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DescribeWorkteam_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeWorkteam) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeWorkteam) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeWorkteam) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeWorkteam) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DescribeWorkteam) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeWorkteam) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeWorkteam) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeWorkteam) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeWorkteam) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeWorkteam) 