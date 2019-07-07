# CreateCodeRepository<a name="API_CreateCodeRepository"></a>

Creates a Git repository as a resource in your Amazon SageMaker account\. You can associate the repository with notebook instances so that you can use Git source control for the notebooks you create\. The Git repository is a resource in your Amazon SageMaker account, so it can be associated with more than one notebook instance, and it persists independently from the lifecycle of any notebook instances it is associated with\.

The repository can be hosted either in [AWS CodeCommit](http://docs.aws.amazon.com/codecommit/latest/userguide/welcome.html) or in any other Git repository\.

## Request Syntax<a name="API_CreateCodeRepository_RequestSyntax"></a>

```
{
   "[CodeRepositoryName](#SageMaker-CreateCodeRepository-request-CodeRepositoryName)": "string",
   "[GitConfig](#SageMaker-CreateCodeRepository-request-GitConfig)": { 
      "[Branch](API_GitConfig.md#SageMaker-Type-GitConfig-Branch)": "string",
      "[RepositoryUrl](API_GitConfig.md#SageMaker-Type-GitConfig-RepositoryUrl)": "string",
      "[SecretArn](API_GitConfig.md#SageMaker-Type-GitConfig-SecretArn)": "string"
   }
}
```

## Request Parameters<a name="API_CreateCodeRepository_RequestParameters"></a>

For information about the parameters that are common to all actions, see [Common Parameters](CommonParameters.md)\.

The request accepts the following data in JSON format\.

 ** [CodeRepositoryName](#API_CreateCodeRepository_RequestSyntax) **   <a name="SageMaker-CreateCodeRepository-request-CodeRepositoryName"></a>
The name of the Git repository\. The name must have 1 to 63 characters\. Valid characters are a\-z, A\-Z, 0\-9, and \- \(hyphen\)\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

 ** [GitConfig](#API_CreateCodeRepository_RequestSyntax) **   <a name="SageMaker-CreateCodeRepository-request-GitConfig"></a>
Specifies details about the repository, including the URL where the repository is located, the default branch, and credentials to use to access the repository\.  
Type: [GitConfig](API_GitConfig.md) object  
Required: Yes

## Response Syntax<a name="API_CreateCodeRepository_ResponseSyntax"></a>

```
{
   "[CodeRepositoryArn](#SageMaker-CreateCodeRepository-response-CodeRepositoryArn)": "string"
}
```

## Response Elements<a name="API_CreateCodeRepository_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [CodeRepositoryArn](#API_CreateCodeRepository_ResponseSyntax) **   <a name="SageMaker-CreateCodeRepository-response-CodeRepositoryArn"></a>
The Amazon Resource Name \(ARN\) of the new repository\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:code-repository/.*` 

## Errors<a name="API_CreateCodeRepository_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

## See Also<a name="API_CreateCodeRepository_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](https://docs.aws.amazon.com/goto/aws-cli/sagemaker-2017-07-24/CreateCodeRepository) 
+  [AWS SDK for \.NET](https://docs.aws.amazon.com/goto/DotNetSDKV3/sagemaker-2017-07-24/CreateCodeRepository) 
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CreateCodeRepository) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CreateCodeRepository) 
+  [AWS SDK for Go \- Pilot](https://docs.aws.amazon.com/goto/SdkForGoPilot/sagemaker-2017-07-24/CreateCodeRepository) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CreateCodeRepository) 
+  [AWS SDK for JavaScript](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sagemaker-2017-07-24/CreateCodeRepository) 
+  [AWS SDK for PHP V3](https://docs.aws.amazon.com/goto/SdkForPHPV3/sagemaker-2017-07-24/CreateCodeRepository) 
+  [AWS SDK for Python](https://docs.aws.amazon.com/goto/boto3/sagemaker-2017-07-24/CreateCodeRepository) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CreateCodeRepository) 