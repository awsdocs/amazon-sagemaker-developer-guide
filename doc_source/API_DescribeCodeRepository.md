# DescribeCodeRepository<a name="API_DescribeCodeRepository"></a>

Gets details about the specified Git repository\.

## Request Syntax<a name="API_DescribeCodeRepository_RequestSyntax"></a>

```
{
   "[CodeRepositoryName](#SageMaker-DescribeCodeRepository-request-CodeRepositoryName)": "string"
}
```

## Request Parameters<a name="API_DescribeCodeRepository_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CodeRepositoryName](#API_DescribeCodeRepository_RequestSyntax) **   <a name="SageMaker-DescribeCodeRepository-request-CodeRepositoryName"></a>
The name of the Git repository to describe\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

## Response Syntax<a name="API_DescribeCodeRepository_ResponseSyntax"></a>

```
{
   "[CodeRepositoryArn](#SageMaker-DescribeCodeRepository-response-CodeRepositoryArn)": "string",
   "[CodeRepositoryName](#SageMaker-DescribeCodeRepository-response-CodeRepositoryName)": "string",
   "[CreationTime](#SageMaker-DescribeCodeRepository-response-CreationTime)": number,
   "[GitConfig](#SageMaker-DescribeCodeRepository-response-GitConfig)": { 
      "[Branch](API_GitConfig.md#SageMaker-Type-GitConfig-Branch)": "string",
      "[RepositoryUrl](API_GitConfig.md#SageMaker-Type-GitConfig-RepositoryUrl)": "string",
      "[SecretArn](API_GitConfig.md#SageMaker-Type-GitConfig-SecretArn)": "string"
   },
   "[LastModifiedTime](#SageMaker-DescribeCodeRepository-response-LastModifiedTime)": number
}
```

## Response Elements<a name="API_DescribeCodeRepository_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [CodeRepositoryArn](#API_DescribeCodeRepository_ResponseSyntax) **   <a name="SageMaker-DescribeCodeRepository-response-CodeRepositoryArn"></a>
The Amazon Resource Name \(ARN\) of the Git repository\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:code-repository/.*` 

 ** [CodeRepositoryName](#API_DescribeCodeRepository_ResponseSyntax) **   <a name="SageMaker-DescribeCodeRepository-response-CodeRepositoryName"></a>
The name of the Git repository\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$` 

 ** [CreationTime](#API_DescribeCodeRepository_ResponseSyntax) **   <a name="SageMaker-DescribeCodeRepository-response-CreationTime"></a>
The date and time that the repository was created\.  
Type: Timestamp

 ** [GitConfig](#API_DescribeCodeRepository_ResponseSyntax) **   <a name="SageMaker-DescribeCodeRepository-response-GitConfig"></a>
Configuration details about the repository, including the URL where the repository is located, the default branch, and the Amazon Resource Name \(ARN\) of the AWS Secrets Manager secret that contains the credentials used to access the repository\.  
Type: [GitConfig](API_GitConfig.md) object

 ** [LastModifiedTime](#API_DescribeCodeRepository_ResponseSyntax) **   <a name="SageMaker-DescribeCodeRepository-response-LastModifiedTime"></a>
The date and time that the repository was last changed\.  
Type: Timestamp

## Errors<a name="API_DescribeCodeRepository_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_DescribeCodeRepository_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/DescribeCodeRepository) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/DescribeCodeRepository) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/DescribeCodeRepository) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/DescribeCodeRepository) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/DescribeCodeRepository) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/DescribeCodeRepository) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/DescribeCodeRepository) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/DescribeCodeRepository) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/DescribeCodeRepository) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/DescribeCodeRepository) 