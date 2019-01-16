# CodeRepositorySummary<a name="API_CodeRepositorySummary"></a>

Specifies summary information about a Git repository\.

## Contents<a name="API_CodeRepositorySummary_Contents"></a>

 **CodeRepositoryArn**   <a name="SageMaker-Type-CodeRepositorySummary-CodeRepositoryArn"></a>
The Amazon Resource Name \(ARN\) of the Git repository\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 2048\.  
Pattern: `arn:aws[a-z\-]*:sagemaker:[a-z0-9\-]*:[0-9]{12}:code-repository/.*`   
Required: Yes

 **CodeRepositoryName**   <a name="SageMaker-Type-CodeRepositorySummary-CodeRepositoryName"></a>
The name of the Git repository\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 63\.  
Pattern: `^[a-zA-Z0-9](-*[a-zA-Z0-9])*$`   
Required: Yes

 **CreationTime**   <a name="SageMaker-Type-CodeRepositorySummary-CreationTime"></a>
The date and time that the Git repository was created\.  
Type: Timestamp  
Required: Yes

 **GitConfig**   <a name="SageMaker-Type-CodeRepositorySummary-GitConfig"></a>
Configuration details for the Git repository, including the URL where it is located and the ARN of the AWS Secrets Manager secret that contains the credentials used to access the repository\.  
Type: [GitConfig](API_GitConfig.md) object  
Required: No

 **LastModifiedTime**   <a name="SageMaker-Type-CodeRepositorySummary-LastModifiedTime"></a>
The date and time that the Git repository was last modified\.  
Type: Timestamp  
Required: Yes

## See Also<a name="API_CodeRepositorySummary_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS SDK for C\+\+](https://docs.aws.amazon.com/goto/SdkForCpp/sagemaker-2017-07-24/CodeRepositorySummary) 
+  [AWS SDK for Go](https://docs.aws.amazon.com/goto/SdkForGoV1/sagemaker-2017-07-24/CodeRepositorySummary) 
+  [AWS SDK for Java](https://docs.aws.amazon.com/goto/SdkForJava/sagemaker-2017-07-24/CodeRepositorySummary) 
+  [AWS SDK for Ruby V2](https://docs.aws.amazon.com/goto/SdkForRubyV2/sagemaker-2017-07-24/CodeRepositorySummary) 