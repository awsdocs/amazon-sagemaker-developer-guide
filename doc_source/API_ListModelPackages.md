# ListModelPackages<a name="API_ListModelPackages"></a>

Lists the model packages that have been created\.

## Request Syntax<a name="API_ListModelPackages_RequestSyntax"></a>

```
{
   "[CreationTimeAfter](#SageMaker-ListModelPackages-request-CreationTimeAfter)": number,
   "[CreationTimeBefore](#SageMaker-ListModelPackages-request-CreationTimeBefore)": number,
   "[MaxResults](#SageMaker-ListModelPackages-request-MaxResults)": number,
   "[NameContains](#SageMaker-ListModelPackages-request-NameContains)": "string",
   "[NextToken](#SageMaker-ListModelPackages-request-NextToken)": "string",
   "[SortBy](#SageMaker-ListModelPackages-request-SortBy)": "string",
   "[SortOrder](#SageMaker-ListModelPackages-request-SortOrder)": "string"
}
```

## Request Parameters<a name="API_ListModelPackages_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CreationTimeAfter](#API_ListModelPackages_RequestSyntax) **   <a name="SageMaker-ListModelPackages-request-CreationTimeAfter"></a>
A filter that returns only model packages created after the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [CreationTimeBefore](#API_ListModelPackages_RequestSyntax) **   <a name="SageMaker-ListModelPackages-request-CreationTimeBefore"></a>
A filter that returns only model packages created before the specified time \(timestamp\)\.  
Type: Timestamp  
Required: No

 ** [MaxResults](#API_ListModelPackages_RequestSyntax) **   <a name="SageMaker-ListModelPackages-request-MaxResults"></a>
The maximum number of model packages to return in the response\.  
Type: Integer  
Valid Range: Minimum value of 1\. Maximum value of 100\.  
Required: No

 ** [NameContains](#API_ListModelPackages_RequestSyntax) **   <a name="SageMaker-ListModelPackages-request-NameContains"></a>
A string in the model package name\. This filter returns only model packages whose name contains the specified string\.  
Type: String  
Length Constraints: Maximum length of 63\.  
Pattern: `[a-zA-Z0-9\-]+`   
Required: No

 ** [NextToken](#API_ListModelPackages_RequestSyntax) **   <a name="SageMaker-ListModelPackages-request-NextToken"></a>
If the response to a previous `ListModelPackages` request was truncated, the response includes a `NextToken`\. To retrieve the next set of model packages, use the token in the next request\.  
Type: String  
Length Constraints: Maximum length of 8192\.  
Required: No

 ** [SortBy](#API_ListModelPackages_RequestSyntax) **   <a name="SageMaker-ListModelPackages-request-SortBy"></a>
The parameter by which to sort the results\. The default is `CreationTime`\.  
Type: String  
Valid Values:` Name | CreationTime`   
Required: No

 ** [SortOrder](#API_ListModelPackages_RequestSyntax) **   <a name="SageMaker-ListModelPackages-request-SortOrder"></a>
The sort order for the results\. The default is `Ascending`\.  
Type: String  
Valid Values:` Ascending | Descending`   
Required: No

## Response Syntax<a name="API_ListModelPackages_ResponseSyntax"></a>

```
{
   "[ModelPackageSummaryList](#SageMaker-ListModelPackages-response-ModelPackageSummaryList)": [ 
      { 
         "[CreationTime](API_ModelPackageSummary.md#SageMaker-Type-ModelPackageSummary-CreationTime)": number,
         "[ModelPackageArn](API_ModelPackageSummary.md#SageMaker-Type-ModelPackageSummary-ModelPackageArn)": "string",
         "[ModelPackageDescription](API_ModelPackageSummary.md#SageMaker-Type-ModelPackageSummary-ModelPackageDescription)": "string",
         "[ModelPackageName](API_ModelPackageSummary.md#SageMaker-Type-ModelPackageSummary-ModelPackageName)": "string",
         "[ModelPackageStatus](API_ModelPackageSummary.md#SageMaker-Type-ModelPackageSummary-ModelPackageStatus)": "string"
      }
   ],
   "[NextToken](#SageMaker-ListModelPackages-response-NextToken)": "string"
}
```

## Response Elements<a name="API_ListModelPackages_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [ModelPackageSummaryList](#API_ListModelPackages_ResponseSyntax) **   <a name="SageMaker-ListModelPackages-response-ModelPackageSummaryList"></a>
An array of `ModelPackageSummary` objects, each of which lists a model package\.  
Type: Array of [ModelPackageSummary](API_ModelPackageSummary.md) objects

 ** [NextToken](#API_ListModelPackages_ResponseSyntax) **   <a name="SageMaker-ListModelPackages-response-NextToken"></a>
If the response is truncated, Amazon SageMaker returns this token\. To retrieve the next set of model packages, use it in the subsequent request\.  
Type: String  
Length Constraints: Maximum length of 8192\.

## Errors<a name="API_ListModelPackages_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_ListModelPackages_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/ListModelPackages) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/ListModelPackages) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/ListModelPackages) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/ListModelPackages) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/ListModelPackages) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/ListModelPackages) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/ListModelPackages) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/ListModelPackages) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/ListModelPackages) 