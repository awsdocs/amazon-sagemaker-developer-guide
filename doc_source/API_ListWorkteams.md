# ListWorkteams<a name="API_ListWorkteams"></a>

Gets a list of work teams that you have defined in a region\. The list may be empty if no work team satisfies the filter specified in the `NameContains` parameter\.

## Request Syntax<a name="API_ListWorkteams_RequestSyntax"></a>

```
{
   "[MaxResults](#SageMaker-ListWorkteams-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListWorkteams-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListWorkteams-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListWorkteams-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListWorkteams-request-SortOrder)": "string"
}
```

## Request Parameters<a name="API_ListWorkteams_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [MaxResults](#API_ListWorkteams_RequestSyntax) **   <a name="SageMaker-ListWorkteams-request-MaxResults"></a>
The maximum number of work teams to return in each page of the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListWorkteams_RequestSyntax) **   <a name="SageMaker-ListWorkteams-request-NameContains"></a>
A string in the work team's name\. This filter returns only work teams whose name contains the specified string\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*`   
Required: No

 ** [NextToken](#API_ListWorkteams_RequestSyntax) **   <a name="SageMaker-ListWorkteams-request-NextToken"></a>
If the result of the previous `ListWorkteams` request was truncated, the response includes a `NextToken`\. To retrieve the next set of labeling jobs, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Pattern: `.*`   
Required: No

 ** [SortBy](#API_ListWorkteams_RequestSyntax) **   <a name="SageMaker-ListWorkteams-request-SortBy"></a>
The field to sort results by\. The default is `CreationTime`\.  
Type: String  
Valid Values:` Name | CreateDate`   
Required: No

 ** [SortOrder](#API_ListWorkteams_RequestSyntax) **   <a name="SageMaker-ListWorkteams-request-SortOrder"></a>
The sort order for results\. The default is `Ascending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

## Response Syntax<a name="API_ListWorkteams_ResponseSyntax"></a>

```
{
   "[NextToken](#SageMaker-ListWorkteams-response-NextToken)": "string",
   "[Workteams](#SageMaker-ListWorkteams-response-Workteams)": [ 
      { 
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
         "[NotificationConfiguration](API_Workteam.md#SageMaker-Type-Workteam-NotificationConfiguration)": { 
            "[NotificationTopicArn](API_NotificationConfiguration.md#SageMaker-Type-NotificationConfiguration-NotificationTopicArn)": "string"
         },
         "[ProductListingIds](API_Workteam.md#SageMaker-Type-Workteam-ProductListingIds)": [ "string" ],
         "[SubDomain](API_Workteam.md#SageMaker-Type-Workteam-SubDomain)": "string",
         "[WorkteamArn](API_Workteam.md#SageMaker-Type-Workteam-WorkteamArn)": "string",
         "[WorkteamName](API_Workteam.md#SageMaker-Type-Workteam-WorkteamName)": "string"
      }
   ]
}
```

## Response Elements<a name="API_ListWorkteams_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [NextToken](#API_ListWorkteams_ResponseSyntax) **   <a name="SageMaker-ListWorkteams-response-NextToken"></a>
If the response is truncated, Amazon SageMaker returns this token\. To retrieve the next set of work teams, use it in the subsequent request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Pattern: `.*` 

 ** [Workteams](#API_ListWorkteams_ResponseSyntax) **   <a name="SageMaker-ListWorkteams-response-Workteams"></a>
An array of `Workteam` objects, each describing a work team\.  
Type: Array of [Workteam](API_Workteam.md) objects

## Errors<a name="API_ListWorkteams_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListWorkteams_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListWorkteams) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListWorkteams) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListWorkteams) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListWorkteams) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/ListWorkteams) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListWorkteams) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListWorkteams) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListWorkteams) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListWorkteams) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListWorkteams) 